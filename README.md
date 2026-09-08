# R4HTTP.R4P

`R4HTTP.R4P` is an independent R4OS protocol module implemented in Zig.

## Package

- Version: `0.1.2`
- Image target: `/R4OS/PROTOCOLS/R4HTTP.R4P`
- Image scope: `full`
- Canonical project manifest: `module.R4MF`

The manifest is the single source of truth for the artifact, imports, image
target, and package metadata.

## Build

On Windows:

    Build.bat

On Linux or macOS:

    ./Build.sh

The build starters resolve the current local R4OS dependency checkouts through
`Settings.R4S`. The URL and hash entries in `build.zig.zon` record the
last verified standalone dependency identities; workspace builds use the
mapped local checkouts.

## Documentation

Detailed German technical notes from the migration are preserved in
`DOCUMENTATION.de.txt`. Source-transfer provenance is recorded in
`PROVENANCE.txt`.

## License

Original R4OS material is licensed under Apache License 2.0. See `LICENSE`
and `NOTICE`. Any repository-specific external material is documented in
`THIRD_PARTY_NOTICES.md`.


HTTP and URL boundaries (0.78.70)
---------------------------------
HTTP URL parsing and request serialization both reject literal control
bytes, whitespace, fragments and incomplete percent escapes in a request
target. The serializer preserves valid percent escapes and percent-encodes
UTF-8 and other non-URI bytes. It also validates directly constructed URL
hosts and custom header names before emitting those fields. Existing
header-value checks remain in place.

URL parts search for a query only before the first fragment delimiter.
URL and Location getters/setters and Origin parsing already use this shared
splitter. Navigation and HTTP redirects now share reference resolution and
path normalization. Only complete dot segments are removed; query and
fragment data, encoded dots and repeated path slashes retain their meaning.
The HTTP resolver still uses caller capacity rather than the browser's
767-byte navigation limit.

A complete response head now returns a specific status/version/header/
length/transfer error. Both response decoders preserve a known header error
across later EOF. Missing terminators remain need_more until EOF; a valid
response with no fields is accepted. The existing incremental body decoder,
connection rules and cache policy are retained.

Six focused host cases cover request bytes, custom headers, redirects,
precise head errors, URL parts and navigation. HTTP checks include both
complete-buffer and streaming entry points. Product builds cover R4HTTP
0.1.4, R4JS 0.54.11, Klickifax 0.29.19, KlickifaxLive 0.2.20 and UpdateService
0.2.19. Recovery 0.1.54 / Menu 0.1.19 uses the same committed HTTP/URL sources
and the updated R4HTTP artifact. These are pure parser checks; no additional
QEMU, network or full browser profile is needed.

References: RFC 9112 sections 3.2, 4 and 5
(https://www.rfc-editor.org/rfc/rfc9112.html), RFC 3986 section 5.2
(https://www.rfc-editor.org/rfc/rfc3986.html), and the WHATWG URL fragment state
(https://url.spec.whatwg.org/#fragment-state).
