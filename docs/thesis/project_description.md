# Engineering Thesis Project Description

## Thesis title

**A multiplatform system for supporting and monitoring strength training with progress analytics, remote coaching, and geolocation-based workout partner matching**

## Scope

The thesis aims to develop a distributed, multiplatform information system using an **offline-first** architecture. It will support strength training, remote coaching, and direct interaction between users of sports facilities, helping address social isolation.

The system comprises a server, a mobile app for trainees, and a web dashboard for coaches. The intended functional modules are outlined below. The [product specification](../product-specification.md) records current product decisions and open questions; in particular, the exact response to geofence events remains to be determined.

### Workout logging and management

The mobile app will record exercises, sets, loads, repetitions, and intensity indicators. Offline-first operation will allow workouts to be logged without a network connection and synchronized after connectivity returns. The module also covers persistent technical notes, machine configuration presets, and asynchronous feedback between trainees and coaches.

### Progress analytics and trends

Analytics will support assessment of training and physique changes by reducing measurement noise in body-weight data, tracking progress toward a stated goal, estimating maximum loads, and analyzing volume and tonnage distribution across muscle groups to identify potential imbalances.

### Coaching management dashboard

The web dashboard will support secure coach–trainee pairing, training-cycle design, and synchronization with the mobile app. Coaches will be able to send plans remotely, review logged workouts and trainee progress, and generate periodic summaries.

### Geofencing and spatial data processing

The location module is intended to use geofencing for passive detection of a user's presence at fitness clubs. This can support starting a workout session and updating the user's activity status at a gym in real time; the exact behavior requires further design.

### Algorithmic workout partner matching

A scoring engine will recommend users who could train together, using feature vectors describing users.

### Coaching offers

A two-way listings module will allow users to filter instructors, trainees to post requests for support, and coaches to submit collaboration offers directly.
