# Dates and Time

<!-- TODO: Review  -->

Introduction to Java Interoperability

Use cases for dates and times

- timestamps on log events
- calendars
- tracking work
  - calculating time between two date stamps


!!! INFO "Java Time available by default"
    `java.time` namespace is available on the class path by default, `require` or `import` expressions are not required.


## Current data time

Create a date and time stamp with the current data and time for the Operating System timezone.

```clojure
(java.time.LocalDateTime/now)
```

A Java time object is returned

```clojure
#object[java.time.LocalDateTime 0xf36f34d "2023-07-13T13:32:22.483261516"]
```

Current date

```clojure
(java.time.LocalDate/now)

;; => #object[java.time.LocalDate 0x5814b4fb "2023-07-13"]
```

Instant

```clojure
(java.time.Instant/now)
;; => #object[java.time.Instant 0x3684d2c0 "2023-07-13T13:02:27.889805413Z"]
```


## Clojure Spec

use Clojure Spec to define a data structure containing a java.time.LocalDate element

```clojure
(spec/def :employee/first-name string?)
(spec/def :employee/last-name string?)
(spec/def :employee/birth-date #(instance? java.time.LocalDate %))

(spec/def :employee/person
  (s/keys :req [:employee/first-name
                :employee/last-name
                :employee/birth-date]))

(def jenny #:ex{:first-name "Jenny"
             :last-name  "Barnes"
             :birth-date (java.time.LocalDate/parse "1910-03-15")})

```

Check to see if Jenny is a valid employee

```clojure
(s/valid? :employee/person jenny)
;; => true
```

