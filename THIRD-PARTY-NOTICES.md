# Third-party notices

TableGrip is built on the open-source components below. Each is used under its own licence; the copyright notices are
reproduced here as those licences require. TableGrip itself is licensed under the MIT License (see `LICENSE`).

| Package | Version | Licence | Copyright | Project |
|---|---|---|---|---|
| PostgresNIO | 1.33.1 | Apache-2.0 | Copyright (c) 2020 Vapor (https://vapor.codes) | https://github.com/vapor/postgres-nio |
| MySQLNIO | 1.8.x | Apache-2.0 | Copyright (c) 2020 Vapor (https://vapor.codes) | https://github.com/vapor/mysql-nio |
| SwiftNIO | 2.x | Apache-2.0 | Copyright (c) 2017-2025 Apple Inc. and the SwiftNIO project authors | https://github.com/apple/swift-nio |
| SwiftNIO SSL | 2.x | Apache-2.0 | Copyright (c) 2018-2025 Apple Inc. and the swift-nio-ssl project authors | https://github.com/apple/swift-nio-ssl |
| SwiftLog | 1.x | Apache-2.0 | Copyright (c) 2018-2025 Apple Inc. and the SwiftLog project authors | https://github.com/apple/swift-log |
| Swift Crypto (brought in by MySQLNIO) | 2.x | Apache-2.0 | Copyright (c) 2019-2025 Apple Inc. and the Swift Crypto project authors | https://github.com/apple/swift-crypto |
| DuckDB and its Swift package | 1.1.3 | MIT | Copyright 2018-2024 Stichting DuckDB Foundation | https://github.com/duckdb/duckdb-swift |
| SQLite | system | Public domain | SQLite is in the public domain | https://sqlite.org |

## Apache-2.0 notices

The components published by Apple Inc. and by the SwiftNIO project are made available under the Apache License,
Version 2.0:

```
Copyright (c) 2017-2025 Apple Inc. and the SwiftNIO project authors
Copyright (c) 2018-2025 Apple Inc. and the swift-nio-ssl project authors
Copyright (c) 2018-2025 Apple Inc. and the SwiftLog project authors
Copyright (c) 2019-2025 Apple Inc. and the Swift Crypto project authors
Copyright (c) 2020 Vapor (https://vapor.codes)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use these files except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific
language governing permissions and limitations under the License.
```

## MIT notice (DuckDB)

```
Copyright 2018-2024 Stichting DuckDB Foundation

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated
documentation files (the "Software"), to deal in the Software without restriction, including without limitation the
rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit
persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the
Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE
WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR
OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```

## The system programs TableGrip uses

| Program | Where it comes from | What it is used for |
|---|---|---|
| `ssh` | macOS (`/usr/bin/ssh`) | Opening an SSH tunnel to a database. TableGrip never reads or stores a host key itself; it lets `ssh` do the checking |
| SQLite | macOS (`libsqlite3`) | Reading and writing SQLite database files |
| Security, CryptoKit, CommonCrypto | macOS | TLS and the SCRAM sign-in for SQL Server, Redis and MongoDB |
