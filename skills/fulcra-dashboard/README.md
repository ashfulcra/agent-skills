# fulcra-dashboard

Turn your own data into something you can actually look at.

Fulcra holds the real-world data you have collected — sleep, movement, listening, whatever you have connected. Useful, but a data store is not a view. This skill builds the view: an interactive dashboard, generated for the data you actually have rather than assembled from a fixed template.

It runs locally, against your own store, and it is customizable in the direction you care about — a chart you want, a comparison nobody anticipated, a layout that suits how you read.

The output is deliberately lightweight: HTML, Alpine.js, plain CSS, with a small Python backend where one is needed. Nothing to build, and no framework to keep up with.

When you want to share something, there is a separate export path that produces a specific previewable directory — so publishing is a deliberate act, and the rest of your data stays where it was.
