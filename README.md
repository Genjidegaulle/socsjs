# socsjs (schedule of classes scraper)
[![npm](https://img.shields.io/npm/v/npm.svg?style=flat-square)](https://www.npmjs.com/package/socsjs)
[![npm](https://img.shields.io/npm/l/express.svg?style=flat-square)](https://www.npmjs.com/package/socsjs)

A scraper that simulates an API for [UCSD's Schedule of Classes](https://act.ucsd.edu/scheduleOfClasses/scheduleOfClassesStudent.htm)  
Many thanks to [andrewthehan](https://github.com/andrewthehan) and [davidmrdavid](https://github.com/davidmrdavid)

## Installation
```
npm install socsjs --save
```

## Usage
```
var socsjs = require('socsjs');
```
### Finding a course
```
var quarter = 'FA16';
var query = 'CSE105';
var timeout = 5000;
socsjs.findCourse(quarter, query, timeout).then(function(result) {
    console.log(result);    // returns a Course
    console.log(result.sections[0].isEnrollable);   // true
}).catch(function(err) {
    console.log(err, 'oops!');
});
```
### Finding courses
```
var quarter = 'FA16';
var queries = ['cse11', 'cse12', 'WCWP10A'];
var timeout = 5000;
socsjs.findCourses(quarter, queries, timeout).then(function(result) {
    console.log(result);    // returns an array of Courses
}).catch(function(err) {
    console.log(err, 'oops!');
});
```
### Searching a department
```
var quarter = 'FA16';
var dept = 'ANTH';
var timeout = 10000;
socsjs.searchDepartment(quarter, dept, timeout).then(function(result) {
    console.log(result);    // returns an array of Courses
}).catch(function(err) {
    console.log(err, 'oops!');
});
```
## Objects
### `Course`
A `Course` object has a String `name` and an array `sections`.  
`sections` is made up of `CourseElements`
### `CourseElement`
A `CourseElement` contains information about a `Course`.
```
var CourseElement = function(type, id, section, days, time, location, teacher, openSeats, seatLimit) {
    this.type = type;           // String describing a course element (eg. 'final', 'discussion')
    this.sectionID = id;        // Null or String of the section's ID (eg. '123456')
    this.section = section;     // Null or String of the section  (eg. 'A01')
    this.days = days;           // String of the days (eg. 'MWF')
    this.time = time;           // String of the time as shown on the Schedule of Classes site
    this.location = location;   // String of the location
    this.teacher = teacher;     // Null or String of LastName,FirstName of teacher
    this.openSeats = openSeats; // Null or Number of how many seats are available
    this.seatLimit = seatLimit; // Null or Number of the course element's class limit

    this.waitlistSize = 0;      // Null or Number of people on the waitlist
    this.isEnrollable = false;  // Boolean for if a class element is enrollable
}

```

## TODOs
- Add filters to get information easily
- Fix error handling
- Set default timeout value
- Format CourseElement fields, such as setting time to have start/end or having proper spacing for teacher names
