# EPG Scripts

Script for tv-guide that includes episode image and excludes episode title if it’s only a date.

```jsx
const axios = require('axios')
const dayjs = require('dayjs')
const utc = require('dayjs/plugin/utc')
const timezone = require('dayjs/plugin/timezone')

dayjs.extend(utc)
dayjs.extend(timezone)

module.exports = {
  site: 'tvguide.com',
  days: 2,
  url: function ({ date, channel }) {
    const [providerId, channelSourceIds] = channel.site_id.split('#')
    const url = `https://internal-prod.apigee.fandom.net/v1/xapi/tvschedules/tvguide/${providerId}/web?start=${date
      .startOf('d')
      .unix()}&duration=1440&channelSourceIds=${channelSourceIds}`

    return url
  },
  async parser({ content }) {
    const programs = []
    const items = parseItems(content)
    for (let item of items) {
      const details = await loadProgramDetails(item)
      programs.push({
        title: item.title,
        sub_title: isDate(details.episodeTitle) ? null : details.episodeTitle, // Check if episodeTitle is a date
        description: details.description,
        season: details.seasonNumber,
        episode: details.episodeNumber,
        rating: parseRating(item),
        categories: parseCategories(details),
        start: parseTime(item.startTime),
        stop: parseTime(item.endTime),
        icon: getEpisodeImage(details) // Add the episode image here
      })
    }

    return programs
  },
  async channels() {
    const providers = [9100001138]

    let channels = []
    for (let providerId of providers) {
      const data = await axios
        .get(
          `https://internal-prod.apigee.fandom.net/v1/xapi/tvschedules/tvguide/serviceprovider/${providerId}/sources/web`
        )
        .then(r => r.data)
        .catch(console.log)

      data.data.items.forEach(item => {
        channels.push({
          lang: 'en',
          site_id: `${providerId}#${item.sourceId}`,
          name: item.fullName
        })
      })
    }

    return channels
  }
}

function parseRating(item) {
  return item.rating ? { system: 'MPA', value: item.rating } : null
}

function parseCategories(details) {
  return Array.isArray(details.genres) ? details.genres.map(g => g.name) : []
}

function parseTime(timestamp) {
  return dayjs.unix(timestamp)
}

function parseItems(content) {
  const data = JSON.parse(content)
  if (!data.data || !Array.isArray(data.data.items) || !data.data.items.length) return []

  return data.data.items[0].programSchedules
}

async function loadProgramDetails(item) {
  item.programDetails = item.programDetails.replace('player1-backend-prod-internal.apigee.net', 'internal-prod.apigee.fandom.net')
  const data = await axios
    .get(item.programDetails)
    .then(r => r.data)
    .catch(err => {
      console.log(err.message)
    })
  if (!data || !data.data || !data.data.item) return {}

  return data.data.item
}

// Function to get the episode image URL
function getEpisodeImage(details) {
  if (Array.isArray(details.images) && details.images.length > 0) {
    // Return the URL of the first image
    return `https://www.metacritic.com/a/img/catalog${details.images[0].bucketPath}`;
  }
  return null; // Return null if no images are found
}

// Function to check if a string is a date in MM-DD-YYYY format
function isDate(str) {
  // Return false if str is null or undefined
  if (!str) return false;
  
  // Match only if the string is exactly in MM-DD-YYYY format without extra characters
  const datePattern = /^(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])-(\d{4})$/;
  const extendedPattern = /^(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])-(\d{4})\s-\d+$/; // Match "MM-DD-YYYY -n"
  
  return datePattern.test(str.trim()) || extendedPattern.test(str.trim());
}

```

[[TVGuide.com](http://TVGuide.com) URLs](EPG%20Scripts%201108c820bc44808586f7f21327c49327/TVGuide%20com%20URLs%201208c820bc4480789c09d6ff1dc1b849.md)