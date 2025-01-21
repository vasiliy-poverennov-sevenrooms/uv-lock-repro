# uv-lock-repro

```shell
uv sync

grep '42' pyproject.toml uv.lock

uv lock
grep '42' pyproject.toml uv.lock
```

With output:
```
% uv sync
Resolved 1 package in 0.28ms
Audited 1 package in 0.05ms
% grep '42' pyproject.toml uv.lock
pyproject.toml:version = "0.42.2"
uv.lock:version = "0.42.1"
% uv lock
Resolved 1 package in 0.22ms
% grep '42' pyproject.toml uv.lock
pyproject.toml:version = "0.42.2"
uv.lock:version = "0.42.1"
```

Before `uv lock --upgrade-package my-lib -vvv`
```
> RUST_LOG=uv=trace uv lock -vvv
    0.000015s DEBUG uv uv 0.5.22 (Homebrew 2025-01-21)
    0.000272s DEBUG uv_workspace::workspace Found workspace root: `/Users/vasiliy/pg/uv-lock-repro`
    0.000284s DEBUG uv_workspace::workspace Adding current workspace member: `/Users/vasiliy/pg/uv-lock-repro`
    0.000344s DEBUG uv::commands::project Using Python request `==3.12.*` from `requires-python` metadata
    0.000421s DEBUG uv_python::discovery Searching for Python ==3.12.* in managed installations or search path
    0.000460s DEBUG uv_python::discovery Searching for managed installations at `/Users/vasiliy/.local/share/uv/python`
    0.000605s DEBUG uv_python::discovery Skipping incompatible managed installation `cpython-3.13.0-macos-aarch64-none`
    0.000611s DEBUG uv_python::discovery Found managed installation `cpython-3.12.8-macos-aarch64-none`
    0.000770s TRACE uv_python::interpreter Cached interpreter info for Python 3.12.8, skipping probing: /Users/vasiliy/.local/share/uv/python/cpython-3.12.8-macos-aarch64-none/bin/python3.12
    0.000779s DEBUG uv_python::discovery Found `cpython-3.12.8-macos-aarch64-none` at `/Users/vasiliy/.local/share/uv/python/cpython-3.12.8-macos-aarch64-none/bin/python3.12` (managed installations)
Using CPython 3.12.8
 uv_client::linehaul::linehaul 
    0.001225s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries 
    0.001560s DEBUG uv_distribution::source Found static `requires-dist` for: /Users/vasiliy/pg/uv-lock-repro/
    0.001691s DEBUG uv_workspace::workspace No workspace root found, using project root
    0.001749s TRACE uv_resolver::lock Static `Requires-Dist` for `my-lib==0.42.1 @ editable+.` is up-to-date
    0.001759s DEBUG uv::commands::project::lock Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 0.63ms
```

```
> RUST_LOG=uv=trace uv lock --upgrade-package my-lib -vvv
    0.000015s DEBUG uv uv 0.5.22 (Homebrew 2025-01-21)
    0.000294s DEBUG uv_workspace::workspace Found workspace root: `/Users/vasiliy/pg/uv-lock-repro`
    0.000314s DEBUG uv_workspace::workspace Adding current workspace member: `/Users/vasiliy/pg/uv-lock-repro`
    0.000381s DEBUG uv::commands::project Using Python request `==3.12.*` from `requires-python` metadata
    0.000484s DEBUG uv_python::discovery Searching for Python ==3.12.* in managed installations or search path
    0.000527s DEBUG uv_python::discovery Searching for managed installations at `/Users/vasiliy/.local/share/uv/python`
    0.000707s DEBUG uv_python::discovery Skipping incompatible managed installation `cpython-3.13.0-macos-aarch64-none`
    0.000712s DEBUG uv_python::discovery Found managed installation `cpython-3.12.8-macos-aarch64-none`
    0.000810s TRACE uv_python::interpreter Querying interpreter executable at /Users/vasiliy/.local/share/uv/python/cpython-3.12.8-macos-aarch64-none/bin/python3.12
    0.042309s DEBUG uv_python::discovery Found `cpython-3.12.8-macos-aarch64-none` at `/Users/vasiliy/.local/share/uv/python/cpython-3.12.8-macos-aarch64-none/bin/python3.12` (managed installations)
Using CPython 3.12.8
 uv_client::linehaul::linehaul 
    0.042610s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries 
    0.042676s DEBUG uv::commands::project::lock Ignoring existing lockfile due to `--upgrade-package`
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=my-lib @ file:///Users/vasiliy/pg/uv-lock-repro
    0.042835s   0ms DEBUG uv_distribution::source Found static `pyproject.toml` for: my-lib @ file:///Users/vasiliy/pg/uv-lock-repro
    0.042916s   0ms DEBUG uv_workspace::workspace No workspace root found, using project root
    0.043040s TRACE uv_requirements::lookahead Performing lookahead for my-lib @ file:///Users/vasiliy/pg/uv-lock-repro
    0.043059s TRACE uv_requirements::lookahead Performing lookahead for my-lib @ file:///Users/vasiliy/pg/uv-lock-repro
 uv_resolver::resolver::solve 
    0.043243s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.12.8
    0.043249s   0ms DEBUG uv_resolver::resolver Solving with target Python version: ==3.12.*
    0.043266s   0ms TRACE uv_resolver::resolver assigned packages: 
    0.043269s   0ms TRACE uv_resolver::resolver Chose package for decision: root. remaining choices: 
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.043324s   0ms DEBUG uv_resolver::resolver Adding direct dependency: my-lib*
    0.043331s   0ms DEBUG uv_resolver::resolver Adding direct dependency: my-lib:dev*
    0.043356s   0ms TRACE uv_resolver::resolver assigned packages: root==0a0.dev0
    0.043359s   0ms TRACE uv_resolver::resolver Chose package for decision: my-lib:dev. remaining choices: my-lib
    0.043364s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of my-lib @ file:///Users/vasiliy/pg/uv-lock-repro (*)
   uv_resolver::resolver::get_dependencies_forking package=my-lib:dev, version=0.42.2
     uv_resolver::resolver::get_dependencies package=my-lib:dev, version=0.42.2
    0.043913s   0ms DEBUG uv_resolver::resolver Adding direct dependency: my-lib:dev==0.42.2
    0.043944s   0ms TRACE uv_resolver::resolver assigned packages: root==0a0.dev0
    0.043948s   0ms TRACE uv_resolver::resolver Chose package for decision: my-lib. remaining choices: my-lib:dev
    0.043952s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of my-lib @ file:///Users/vasiliy/pg/uv-lock-repro (*)
   uv_resolver::resolver::get_dependencies_forking package=my-lib, version=0.42.2
     uv_resolver::resolver::get_dependencies package=my-lib, version=0.42.2
    0.043983s   0ms TRACE uv_resolver::resolver assigned packages: root==0a0.dev0, my-lib==0.42.2
    0.043985s   0ms TRACE uv_resolver::resolver Chose package for decision: my-lib:dev. remaining choices: 
    0.044605s   1ms DEBUG uv_resolver::resolver Searching for a compatible version of my-lib @ file:///Users/vasiliy/pg/uv-lock-repro (==0.42.2)
   uv_resolver::resolver::get_dependencies_forking package=my-lib:dev, version=0.42.2
     uv_resolver::resolver::get_dependencies package=my-lib:dev, version=0.42.2
    0.044623s   1ms TRACE uv_resolver::resolver assigned packages: root==0a0.dev0, my-lib==0.42.2, my-lib:dev==0.42.2
    0.044626s   1ms DEBUG uv_resolver::resolver::batch_prefetch Tried 1 versions: my-lib 1
    0.044632s   1ms DEBUG uv_resolver::resolver all marker environments resolution took 0.001s
    0.044661s   1ms TRACE uv_resolver::resolver Resolution: ResolverEnvironment { kind: Universal { initial_forks: [], markers: true, include: {}, exclude: {} } }
    0.045316s   2ms TRACE uv_resolver::resolver Resolution edge: ROOT -> my-lib
    0.045321s   2ms TRACE uv_resolver::resolver Resolution edge:     0a0.dev0 -> 0.42.2 (group: dev)
    0.045323s   2ms TRACE uv_resolver::resolver Resolution edge: ROOT -> my-lib
    0.045326s   2ms TRACE uv_resolver::resolver Resolution edge:     0a0.dev0 -> 0.42.2
Resolved 1 package in 2ms
vasiliy@vasya-mbp2 ~/p/uv-lock-repro (main)> RUST_LOG=uv=trace uv lock --upgrade-package my-lib -vvv
    0.000020s DEBUG uv uv 0.5.22 (Homebrew 2025-01-21)
    0.000634s DEBUG uv_workspace::workspace Found workspace root: `/Users/vasiliy/pg/uv-lock-repro`
    0.000661s DEBUG uv_workspace::workspace Adding current workspace member: `/Users/vasiliy/pg/uv-lock-repro`
    0.000731s DEBUG uv::commands::project Using Python request `==3.12.*` from `requires-python` metadata
    0.000831s DEBUG uv_python::discovery Searching for Python ==3.12.* in managed installations or search path
    0.000873s DEBUG uv_python::discovery Searching for managed installations at `/Users/vasiliy/.local/share/uv/python`
    0.001013s DEBUG uv_python::discovery Skipping incompatible managed installation `cpython-3.13.0-macos-aarch64-none`
    0.001018s DEBUG uv_python::discovery Found managed installation `cpython-3.12.8-macos-aarch64-none`
    0.001084s TRACE uv_python::interpreter Querying interpreter executable at /Users/vasiliy/.local/share/uv/python/cpython-3.12.8-macos-aarch64-none/bin/python3.12
    0.040591s DEBUG uv_python::discovery Found `cpython-3.12.8-macos-aarch64-none` at `/Users/vasiliy/.local/share/uv/python/cpython-3.12.8-macos-aarch64-none/bin/python3.12` (managed installations)
Using CPython 3.12.8
 uv_client::linehaul::linehaul 
    0.040899s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries 
    0.040959s DEBUG uv::commands::project::lock Ignoring existing lockfile due to `--upgrade-package`
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=my-lib @ file:///Users/vasiliy/pg/uv-lock-repro
    0.041135s   0ms DEBUG uv_distribution::source Found static `pyproject.toml` for: my-lib @ file:///Users/vasiliy/pg/uv-lock-repro
    0.041208s   0ms DEBUG uv_workspace::workspace No workspace root found, using project root
    0.041334s TRACE uv_requirements::lookahead Performing lookahead for my-lib @ file:///Users/vasiliy/pg/uv-lock-repro
    0.041353s TRACE uv_requirements::lookahead Performing lookahead for my-lib @ file:///Users/vasiliy/pg/uv-lock-repro
 uv_resolver::resolver::solve 
    0.041476s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.12.8
    0.041482s   0ms DEBUG uv_resolver::resolver Solving with target Python version: ==3.12.*
    0.041500s   0ms TRACE uv_resolver::resolver assigned packages: 
    0.041503s   0ms TRACE uv_resolver::resolver Chose package for decision: root. remaining choices: 
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.041560s   0ms DEBUG uv_resolver::resolver Adding direct dependency: my-lib*
    0.041567s   0ms DEBUG uv_resolver::resolver Adding direct dependency: my-lib:dev*
    0.041698s   0ms TRACE uv_resolver::resolver assigned packages: root==0a0.dev0
    0.041703s   0ms TRACE uv_resolver::resolver Chose package for decision: my-lib:dev. remaining choices: my-lib
    0.041707s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of my-lib @ file:///Users/vasiliy/pg/uv-lock-repro (*)
   uv_resolver::resolver::get_dependencies_forking package=my-lib:dev, version=0.42.2
     uv_resolver::resolver::get_dependencies package=my-lib:dev, version=0.42.2
    0.041723s   0ms DEBUG uv_resolver::resolver Adding direct dependency: my-lib:dev==0.42.2
    0.041731s   0ms TRACE uv_resolver::resolver assigned packages: root==0a0.dev0
    0.041733s   0ms TRACE uv_resolver::resolver Chose package for decision: my-lib. remaining choices: my-lib:dev
    0.042352s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of my-lib @ file:///Users/vasiliy/pg/uv-lock-repro (*)
   uv_resolver::resolver::get_dependencies_forking package=my-lib, version=0.42.2
     uv_resolver::resolver::get_dependencies package=my-lib, version=0.42.2
    0.042376s   0ms TRACE uv_resolver::resolver assigned packages: root==0a0.dev0, my-lib==0.42.2
    0.042378s   0ms TRACE uv_resolver::resolver Chose package for decision: my-lib:dev. remaining choices: 
    0.042382s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of my-lib @ file:///Users/vasiliy/pg/uv-lock-repro (==0.42.2)
   uv_resolver::resolver::get_dependencies_forking package=my-lib:dev, version=0.42.2
     uv_resolver::resolver::get_dependencies package=my-lib:dev, version=0.42.2
    0.043212s   1ms TRACE uv_resolver::resolver assigned packages: root==0a0.dev0, my-lib==0.42.2, my-lib:dev==0.42.2
    0.043217s   1ms DEBUG uv_resolver::resolver::batch_prefetch Tried 1 versions: my-lib 1
    0.043219s   1ms DEBUG uv_resolver::resolver all marker environments resolution took 0.002s
    0.043238s   1ms TRACE uv_resolver::resolver Resolution: ResolverEnvironment { kind: Universal { initial_forks: [], markers: true, include: {}, exclude: {} } }
    0.043243s   1ms TRACE uv_resolver::resolver Resolution edge: ROOT -> my-lib
    0.043246s   1ms TRACE uv_resolver::resolver Resolution edge:     0a0.dev0 -> 0.42.2 (group: dev)
    0.043248s   1ms TRACE uv_resolver::resolver Resolution edge: ROOT -> my-lib
    0.043821s   2ms TRACE uv_resolver::resolver Resolution edge:     0a0.dev0 -> 0.42.2
Resolved 1 package in 3ms
Updated my-lib v0.42.1 -> v0.42.2
```

After `uv lock --upgrade-package my-lib -vvv`:
```
> RUST_LOG=uv=trace uv lock -vvv
    0.000007s DEBUG uv uv 0.5.22 (Homebrew 2025-01-21)
    0.000166s DEBUG uv_workspace::workspace Found workspace root: `/Users/vasiliy/pg/uv-lock-repro`
    0.000172s DEBUG uv_workspace::workspace Adding current workspace member: `/Users/vasiliy/pg/uv-lock-repro`
    0.000201s DEBUG uv::commands::project Using Python request `==3.12.*` from `requires-python` metadata
    0.000243s DEBUG uv_python::discovery Searching for Python ==3.12.* in managed installations or search path
    0.000262s DEBUG uv_python::discovery Searching for managed installations at `/Users/vasiliy/.local/share/uv/python`
    0.000333s DEBUG uv_python::discovery Skipping incompatible managed installation `cpython-3.13.0-macos-aarch64-none`
    0.000336s DEBUG uv_python::discovery Found managed installation `cpython-3.12.8-macos-aarch64-none`
    0.000420s TRACE uv_python::interpreter Cached interpreter info for Python 3.12.8, skipping probing: /Users/vasiliy/.local/share/uv/python/cpython-3.12.8-macos-aarch64-none/bin/python3.12
    0.000425s DEBUG uv_python::discovery Found `cpython-3.12.8-macos-aarch64-none` at `/Users/vasiliy/.local/share/uv/python/cpython-3.12.8-macos-aarch64-none/bin/python3.12` (managed installations)
Using CPython 3.12.8
 uv_client::linehaul::linehaul 
    0.000624s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries 
    0.000770s DEBUG uv_distribution::source Found static `requires-dist` for: /Users/vasiliy/pg/uv-lock-repro/
    0.000824s DEBUG uv_workspace::workspace No workspace root found, using project root
    0.000845s TRACE uv_resolver::lock Static `Requires-Dist` for `my-lib==0.42.2 @ editable+.` is up-to-date
    0.000851s DEBUG uv::commands::project::lock Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 0.27ms
```
