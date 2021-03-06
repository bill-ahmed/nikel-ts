# Models
This section contains information on each of the models provided by the Nikel API. All models inherit
from `Base`, and only specify information that is unique to them. Currently, that information 
tells the `Axios` service where to send queries.

## Usage
All queries use a syntax similar to the MongoDB query selectors. The following operations are 
supported:
1. `$eq` -- Equality
2. `$ne` -- Inequality
3. `$lt` -- Less than
4. `$lte` -- Less than or equal to
5. `$gt` -- Greater than
6. `$gte` -- Greater than or equal to
7. `$sw` -- Starts with
8. `$ew` -- Ends with

When an operation is not provided, the _equality_ operator is assumed.

To query with any of the above, simply pass in a hash that uses them via a `where()` clause. 
They can be as deeply nested and compounded as desired.

### Examples

(a) To get a course by code:
```typescript
import { Courses } from nikel;

// Course for id='CSCA08'
Courses.where({ id: 'CSCA08' }).get();

// More verbously
Courses.where({ id: { $eq: 'CSCA08' } }).get();
```
(b) To access a nested field
```typescript
import { Buildings } from nikel;

Buildings.where({ address: { postal: 'M5S 1A1' } }).get();
```

(c) Change query based on a condition
```typescript
import { Foods } from nikel;

let my_food = Foods.where({ campus: 'St. George' });

if(its_my_birthday()) {
    my_food.where({ coordinates: { longitude: { $lt: 50 } } });
}

my_food.get();   // All food places at St. George with longitude less than 50
```

(d) Combining various operations
```typescript
import { Buildings } from nikel;

let my_courses = Buildings.where({
    id: { $sw: '13' },
    code: 'NR',
    name: { $ew: 'II' },
    coordinates: {
        latitude: { $gt: 30 },
        longitude: -79.40078
    }
});
```
