## @musement/iso-duration

github actions tests

JS's library created to provide easy API for working with time duration.  
Based on ISO 8601.  
Inspired by [HumanizeDuration](https://github.com/EvanHahn/HumanizeDuration.js) and [ISO8601-duration](https://github.com/tolu/ISO8601-duration).

#### Installation: 
```
npm i @musement/iso-duration --save
```

#### Quick start:
```js
// Import isoDuration and locales
import { isoDuration, en, pl, it } from '@musement/iso-duration'

// Setup locales
//   key - string you want to use in `humanize` function
//   value - IsoDuration i18n object.
isoDuration.setLocales(
  {
    en,
    pl,
    it,
  },
  {
    fallbackLocale: 'en',
  }
)

//Create duration object
const duration = isoDuration("P8DT30M")
// OR
const duration = isoDuration({
  days: 8,
  minutes: 30
})

//Get JS duration object
console.log(duration.parse())
// {
//   weeks: 0,
//   years: 0,
//   months: 0,
//   days: 8,
//   hours: 0,
//   minutes: 30,
//   seconds: 0
// }

//Get duration ISO string
console.log(duration.toString())
// P8DT30M

//Humanize duration 
console.log(duration.humanize('en'))
// 8 days 30 minutes


``` 

#### API:
Duration object supports:
 * [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601#Durations) duration string (`PnYnMnDTnHnMnS`, `PnW`)
 * JS objects:
```js
{
  "weeks": Number,
  "years": Number,
  "months": Number,
  "days": Number,
  "hours": Number,
  "minutes": Number,
  "seconds": Number,
}
```

Example:

```js
import { isoDuration } from '@musement/iso-duration@'

const durationFromString = isoDuration("P8DT30M")
const durationFromObj = isoDuration({
  days: 8,
  minutes: 30,
})
```

### i18n
All languages which are expected to be available in `.humanize` method needs to be loaded using `isoDuration.setLocales`
function. Currently, library provides support for languages listed under `/src/locales`.

```js
import { isoDuration, en, pl, it } from '@musement/iso-duration'

isoDuration.setLocales({
  en,
  pl,
  it,
  'en-GB': en,
  'en-US': en,  
})
```

#### Configuration
`isoDuration.setLocales` can take an `option` object as second argument (optional):

| Key                    | Type     | Description                                                   | Default
| ---------------------- | -------- |-------------------------------------------------------------- | -----------
| `fallbackLocale`       | String   | fallback locale used when an invalid locale is passed to `IsoDuration.humanize` | `undefined`


### IsoDuration object:
* `parse()` return `DurationObj` with all time periods represented as JS object
```js
console.log(isoDuration("P8T30M").parse())
// {
//   weeks: 0,
//   years: 0,
//   months: 0,
//   days: 8,
//   hours: 0,
//   minutes: 30,
//   seconds: 0
// }
```

* `toString()` returns ISO_8601 string:
```js
console.log(isoDuration("P8T30M").toString())
// P8T30M
```

* `humanize(lang: string, config?: HumanizeConfig)` returns duration in human readable way:   
⚠ Warning ️⚠ - used `lang` needs to be previously added to the library using `isoDuration.setLocales`
```js
console.log(isoDuration("P8T30M").humanize('en'))
// 8 hours 30 minutes
```

`HumanizeConfig` Object:
```js
{
  largest: number
}
```
`largest` - Reduce number of humanized to N the largest units:
```js
console.log(
  isoDuration({
    years: 2,
    months: 0,
    days: 8,
    hours: 20,
    minutes: 30,
    seconds: 15
  }).humanize('en', { largest: 2 })
)
// 2 years 8 days
```

* `normalize(date?: Date)` returns normalized IsoDuration object:
```js
console.log(isoDuration("PT90M").normalize().parse())
// PT1H30M
```

This method takes an optional argument `date` which defines start of duration. It's used to correctly normalize number of days basing on corresponding month's length.   
If it's not present normalize function will use current date (`new Date()`) instead.

Month with 31 days:
```js
console.log("Duration:", isoDuration("P31D").normalize(new Date(2020, 0, 1)).humanize('en'))
//Duration: 31 days
```

Month with 29 days:
```js
console.log("Duration:", isoDuration("P31D").normalize(new Date(2020, 1, 1)).humanize('en'))
//Duration: 1 month 2 days
```

### Credits
[EvanHahn](https://github.com/EvanHahn) - author of [HumanizeDuration](https://github.com/EvanHahn/HumanizeDuration.js)  
[tolu](https://github.com/tolu) author of [ISO8601-duration](https://github.com/tolu/ISO8601-duration)
