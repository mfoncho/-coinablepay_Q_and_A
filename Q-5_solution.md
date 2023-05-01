#### Elixir backend interview questions

- Write a GenServer process that recieves order events and sends mails and guarantee at lest once delivery

- Write a function with the spec below that calls an external binary `convert /tmp/source.jpg -thumbnail 100x100 -gravity center /tmp/destination.jpg` source and desination paths should be replaced with args passed to the function
    - `@spec resize(source_path::binary(), destination_path::binary()) {:ok, binary()} | {:error, binary()}`

- Write a function with documentation for a function that returns the sum of two arguments a and b
    - arguments must be integers
    - arguments must be less than 100
    - arguments must be greater than zero

- Have you implemented a liveview component? if yes what did the component do?

- Name one advantage of elixir umbrella project

- An application API Controller recieves a map from a post request with string keys, and the context api expects a maps with atom keys, how will you pass a valid map to the context api

- Elixir Superviors have restart strategies, provide a use case for each strategy why you think the stragegy best fits said use case

- What is the difference bewteen the behaviour of a process defined with `use GenServer, restart: :transient` and `use GenServer, restart: :permanent`

- You have and idea for a new feature and the project lead tells you to create prototype  on a feature branch, you draw up your plans and write a spec,
    - Do you write tests before implementation? if Yes why?
    - Do you do implementation before the writing the test? if Yes why?

- What other programing languages are you current proficient at

- Do you use git or mercurial for your personal projects

