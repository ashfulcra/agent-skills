# fulcra-ingest

Gets the data you already own out of a download folder and into somewhere it is useful.

Most services will give you your data if you ask — Spotify, Netflix, and the rest will email you an export. What arrives is a zip of CSVs and JSON in whatever shape that company happened to pick, which is why those exports usually get downloaded once and never opened again.

This skill does the unglamorous part. Point it at an export you have uploaded to your Fulcra file store, and it works out what the file actually contains, maps it onto Fulcra annotations, and ingests it — preserving where each record came from and how confident the match was.

The result is that a Netflix export stops being a file and starts being data you can query alongside everything else you have, by any agent you have given access.

Supports multiple export formats, and keeps provenance so you can always tell what came from where.
