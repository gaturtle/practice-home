# OpenAPI file as the cross-repo contract

`reading-list-api` generates `openapi.json` from its Zod route schemas and commits it. `reading-list-web` generates its typed client from that file in the sibling clone (`../reading-list-api/openapi.json`). We picked this over Hono RPC (which needs the frontend to import backend types, coupling the repos through a package or link) and over hand-copied types (which drift). The committed spec makes the boundary explicit and turns a cross-repo change into a mechanical sequence: change the API, regenerate the spec, regenerate the client, fix type errors.
