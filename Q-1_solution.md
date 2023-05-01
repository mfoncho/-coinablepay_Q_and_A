### Store Order 

#### Assumptions
- Backend api is REST
- Backend api is versioned
- Backend team has a documention structure
- Development team is familiar with version control git or mercurial
- Backend library used is Elixir Phoenix
- Mechant has a Store domain 
- Frontend built with Typescript
- Frontend is developed with npm workspaces
    - website built with nextjs package
    - UI components seperate package with storybooks integration
    - site client package (http client used for website api request like placing orders)
    - store client package (http client used for store dashboard)

- Order dashboard is built with React project like Nextjs used in frontend

#### Milestones
- Discuss Business logic and constraints like referals and coupons etc.
- Discuss lagal constraints like GDPR and data retension policies
- Select ui wireframe
- Develope Api boundries specs for
    - Context app and Phoenix app
    - Phoenix app and http clients
    - http clients and ui components

- Implement Core order logic
    - Develope Mechant Store data schemas for Order and OrderMetadata
    - Implement the Store context and tests for the Mechant Store actions
    - Tag stable release for umbrella projects

- Implement Mechant orders dashboard API
    - Add Route, Controller and Views for mechant orders
    - Test and fuzz dashboard backend rest Api
    - Add `async` getter methods for the Mechant's dashboard store orders in the store http client
    - Test mechant dashboard store http client
    - Test client against the backend api
    - Tag stable release with api version

- Implement Customer orders API
    - Add Route, Controller and Views for customer orders
    - Test and fuzz customer orders backend rest Api
    - Implement `async` methods to place and view orders for Customer in the site http client
    - Test site http client
    - Test client against the backend api
    - Tag stable release with api version

- Select Ordering UI with customer input area
    - Implement ui components
    - Test ui components
    
- Select Mechant Dashboard UI
    - Implement dashboard ui components
    - Test ui components

- Integrate order UI components into site
- Integrate dashboard UI components into mechant dashboard ui console
- Test and fuzz end to end integration
- Prelease (roll out to select clients)
- Full release (roll out too all clients)

