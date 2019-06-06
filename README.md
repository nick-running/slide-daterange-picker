# slide-daterange-picker

## install
```
npm install slide-daterange-picker --save
```

## usage
```
<template>
    <div>
      {{dateRange}}
      <slider @on-date-range-change="dateRange=$event"
        :total-date-range="['2019-6-4 00:00:00', '2019-6-4 23:00:01']"
        format="YYYY-MM-DD HH:mm:ss"
        :date-range="['2019-6-4 00:12:00', '2019-6-4 00:23:00']">
        <div>custom content</div>
      </slider>
    </div>
</template>

<script>
  export default {
    name: "app",
    data() {
      return {
        dateRange: null
      }
    },
  }
</script>
```
#### Props
#### height
  Type: `Number`<br>
  Required: `false`<br>
  Default: `30`
  
Set picker height, also can use style attribute to set in-line style height.

#### format
  Type: `String`<br>
  Required: `false`<br>
  Default: `false`

Define the string template base on vue-moment library, format is same to moment.

#### totalDateRange
  Type: `Array`<br>
  Required: `true`<br>
 
Define total date range.
* `0` - startDate
* `1` - endDate

#### dateRange
  Type: `Array`<br>
  Required: `true`<br>
 
Define date range.
* `0` - startDate
* `1` - endDate

### Events

#### on-date-range-change

Required: `false`<br>
Parameters: `-`

Called when the slider bar slide.

```html
<slider @on-date-range-change="onDateRangeChange"/>
```
