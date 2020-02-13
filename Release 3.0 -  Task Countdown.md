# Release 3.0

Release 3.0 has two primary goals:

- Move forward to be compatible with OTP 20 (without backwards compatibility);
- Look to deprecate features to simplify the long-term structure of Riak.

The aim is to provide confidence to current Riak customers that Riak will be efficiently maintainable for at least the next o(10) years, but also make Riak easier to adopt within existing scale-out Erlang/Elixir environments.


## Background to the work on 3.0

The work on release 3.0 builds on the efforts of multiple people who have contributed to OTP20 on Riak before, in particular Ted Burghart and Andrew Thompson.  The release is now being worked on by a joint team, through a series of workshop, pulling in experience from:

- Quviq;
- NHS;
- Airelogic;
- bet365;
- plus other key individuals - e.g. Christopher Meiklejohn, Antonio Nikishaev.

## Workshop 2 - 8th May to 10th May 2019

### Progress at end of Workshop

- Repeatable compilation, and release/startup of Riak on OTP 20 with all components (i.e. including yokozuna);

- A temporary solution to allow for `make devrel` to create an 8 node cluster with appropriate variable substitution;

- Merge in of major 2.9 code uplifts in 3.0 (riak_kv, riak_core) - 3.0 now to build on 2.9;

- Repeatable capability to run individual and groups of `riak_test` tests;

- Analysis of initial riak_test failures.

### Key Challenges Remaining

Where future challenges have changed since previous workshop, items are **now in bold**.

a. Achieve a complete unit/eqc test pass rate across riak (using the locked dependencies)

**b. Complete pass rate across riak_test tests**

c. Address dialyzer issues, without depending on line-referenced ignore files.  Note there is unreleased work from Basho to assist with this work.  Replace use of nowarn export-all in non-test code.

**d. Outstanding work on script exits, and ensuring make devrel is robust and repeatable**

**e. Complete merge of all repos changed in 2.9.  Also merge in 2.9.1**

f. Develop a better basic test for the riak parent.  Potentially port in Goncalo Tomas's `riak_tests` into riak so that we can run the `riak_test` `vape` tests as a `rebar ct` test option.  Determine an overnight CI strategy for `develop-3.0`.

**g. To complete yokozuna integration, remove hard dependency via** https://github.com/basho/riak_kv/pull/1571


h. Third party repo review e.g. webmachine, mochiweb, poolboy, lager - fix versions, and agree strategy.  Also look at removing "minor" repos that are used sparingly (e.g. kvc), or are related to test issue (e.g. sidejob)

i. Investigate better isolation of components through the build process so that the nucleus of Riak can be deployed without "optional" components - e.g. ensemble, yokozuna, nif-backends, legacy-repl.

j. Non-functional testing for performance/throughput comparison to `develop-2.9`.

k. Testing of migration with repeatable instructions (ideally for a single machine), between `develop-2.9`/OTP16 to `develop-3.0`/OTP20.

l. Fully document feature incompatibilities e.g. Tictac AAE, yokozuna, multi-backend, riak_ensemble.  Removing redacted deprecation notices.

m. Priority `gen_statem` uplifts - specifically `riak_kv_get_fsm` and `riak_kv_put_fsm`.

__n. Add travis CI scripts for major repos - should aim to cover at least eunit tests and dialyzer.  Potential to concurrently have a Quickcheck CI service validate PRs (or overnight builds)__

## Workshop 1 - 6th March to 8th March 2019

### Progress at end of workshop

- Repeatable compilation of Riak on OTP 20 (without yokozuna) on multiple platforms.

- Repeatable compilation of riak_test on OTP 20.

- Analysis of unit and eqc test failures across riak.

- Initial test fixes to riak_kv and riak_core

### Broad Challenges Going Forward - Towards releasing Riak 3.0

a. Achieve a complete unit/eqc test pass rate across riak (using the locked dependencies)

b. Run and pass riak_test tests

c. Address dialyzer issues, without depending on line-referenced ignore files.  Note there is unreleased work from Basho to assist with this work.  Replace use of nowarn export-all in non-test code.

d. Resolve long-term release tooling approach (e.g. use of `rebar.lock`, use of make vs rebar3, supporting devrel, block use of rebar).

e. Merge in `develop-2.9`, and repeat [a-c]

f. Develop a better basic test for the riak parent.  Potentially port in Goncalo Tomas's `riak_tests` into riak so that we can run the `riak_test` `vape` tests as a `rebar ct` test option.  Determine an overnight CI strategy for `develop-3.0`.

g. Merge in yokozuna `develop-3.0` and prove integrated riak/yokozuna on OTP 20

h. Third party repo review e.g. webmachine, mochiweb, poolboy, lager - fix versions, and agree strategy.  Also look at removing "minor" repos that are used sparingly (e.g. kvc), or are related to test issue (e.g. sidejob)

i. Investigate better isolation of components through the build process so that the nucleus of Riak can be deployed without "optional" components - e.g. ensemble, yokozuna, nif-backends, legacy-repl.

j. Non-functional testing for performance/throughput comparison to `develop-2.9`.

k. Testing of migration with repeatable instructions (ideally for a single machine), between `develop-2.9`/OTP16 to `develop-3.0`/OTP20.

l. Fully document feature incompatibilities e.g. Tictac AAE, yokozuna, multi-backend, riak_ensemble.  Removing redacted deprecation notices.

m. Priority `gen_statem` uplifts - specifically `riak_kv_get_fsm` and `riak_kv_put_fsm`.




### Outstanding Challenges prior to Workshop 2 (8th May)

Task | Status
:-------------------------|:-------------------------:|
Continue work on unit/eqc test pass rates | progressed - quviq
First merge in of develop-2.9 | progressed - mas/mcx
Work on relx scripts for running riak_test | progressed - mcx
First pass at running riak_test, analysis of failures and reasons (needs `make devrel`) | progressed at workshop 2
First pass at dialyzer, analysis of failures and reasons | no progress
Write a shot developer guide for working on 3.0 | no progress

### Know riak_test issues

TEST | REASON | PROPOSED ACTION | STATUS
:-- | :-- | :-- | --
basic_command_line | Exit code from scripts | mcx investigating across all scripts | Fail
bucket_props_validations | issue with druuid | remove druuid module | Pass - 10/05/19
bucket_types | js map/reduce (no longer supported) | replace with erlang map/reduce | Fail
coverage_participation | intermittent issue with final stage (proving node included in coverage plans when participation re-enabled) | Not known | Fail
http_bucket_types | js map/reduce (no longer supported) | replace with erlang map/reduce | Fail
partition_repair | enoent on spam files | spam files not unzipped correctly - local issue | Pass - 10/05/18
pb_security | Issue with hostname validation (new to OTP20?) | Not known | Fail
verify_riak_stats | Repos removed from riak expected in stats, more repos reporting than previously (not necessarily new ones) | Some apps have been removed (js map/reduce and riak_control) - some pre-existing libraries have been added to app.src so although they are not new they are now reported.  Test should reflect new reality | Pass - 10/05/19

### Known unit/property test issues

REPO | NODE | COMMIT | NOTES | QuickCheck |
-- | -- | -- | -- | -- |
basho_stats | 0  | develop-3.0 | neither EUnit tests nor QC tests run because missing_R_executable file |
bear | 0 | develop-3.0 | use upstream |
canola | 0 | develop-3.0 | |
cluster_info | 0 | develop-3.0 | |
ebloom | 0 | develop-3.0 | |
edown | 0 | develop-3.0 | |
eleveldb | 0  | develop-3.0 | |
eper | 0 | develop-3.0 | use upstream? 83 EUnit tests passed, 0 failures, 51% coverage. | No eqc tests |
erlang_js | 0 | develop-3.0 | removed? |
erlydtl | 0 | develop-3.0 | |
eunit_formatters | 0 | develop-3.0 | use upstream |
fuse | 0 | develop-3.0 | |
getopt | 0 | develop-3.0 | |
goldrush | 0 | develop-3.0 | use upstream |
hamcrest | 0 | develop-3.0 | |
ibrowse | 0 | develop-3.0 | |
kvc | 0 | develop-3.0 | use upstream |
meck | 0 | develop-3.0 | |
mochiweb | 0 | develop-3.0 | need to consider upstream |
neotama | 0 | develop-3.0 | |
node_package | 0 | develop-3.0 | |
pbkdf2 | 0 | develop-3.0 | 46 EUnit test passed, 0 failures |
poolboy | 0 | develop-3.0 | merge with upstream. (devinus: all 20 EUnit tests passed, 62% coverage. 2 EQC properties passed, 71.5% coverage. kyorai: all 12 EUnit tests passed, 76% coverage. EQC did not run at first but after some fixing (not complicated) the same 2 EQC properties passed, 73% coverage) |
proper | 0 | develop-3.0 | |
ranch | 0 | develop-3.0 | use upstream |
rebar_lock_deps_plugin | 0 | develop-3.0 | provided by rebar3 |
riak_dt | 0 | develop-3.0 | dialyzer needs work. All 105 EUnit tests passed, 76% coverage. | 20 properties passed.
sidejob | 0 | develop-3.0 | QuickCheck tests fail due to known race, appears more likely to fail in OTP20. No EUnit tests. | __REVISIT QuickCheck tests__
stdlib2 | 0 | develop-3.0 | |
syslog | 0 | develop-3.0 | use upstream |
folsom | 1 | develop-3.0 | use upstream |
hyper | 1 | develop-3.0 | one unit test fails, dialyzer |
lager | 1 | develop-3.0 | use erlang-lager fork |
lager_syslog | 1 | develop-3.0 | use erlang-lager fork |
parse_trans | 1 | develop-3.0 |  |
protobuffs | 1 | n/a | rebar_gpb_plugin now used with  tsloughter/riak_pb_msgcodegen  |
riak_auth_mods | 1 | develop-3.0 |  |
setup | 1 | develop-3.0 | |
sext | 1 | develop-3.0 |  | 24 properties passed
webmachine | 1 |  develop-3.0 | use upstream - will need to add support for configurable receive buffer https://github.com/webmachine/webmachine/issues/299
cuttlefish | 2 | __??? develop-3.0 ???__| There is no develop-3.0 branch in bash repo. In riak_sysmon we point to `rebar3` branch. In rebar3 branch 291 tests pass | 1 property passes
exometer_core | 2 | develop-3.0 | use upstream  |
merge_index | 2 | - | can be removed?  |
riak_ensemble | 2 | develop-3.0 | dialyzer fails, copied in files from riak_test. 19 EUnit test passed, nothing about failures, 71% coverage.  | __REVISIT__ 1 property in `eqc/sc.erl`. It passes, but unclear what it tests. Has hard-coded paths in property, e.g. `code:add_path("/Users/jtuple/basho/riak_test.master/deps/riakc/ebin")`  |
riak_pb | 2 | __develop-3.0-lower__ | dialyzer isn't clean & I had to make a rebar3 plugin that's currently hosted under my own GH account | One property covers 67% of riak_pb_codec
riaknostic | 2 | - | can be removed? | |
bitcask | 3 | develop-3.0 | depends on 'rebar3' branch of cuttlefish, dialyzer not clean | __REVISIT__
clique | 3 | develop-3.0 | depends on 'rebar3' branch of cuttlefish | |
riak_repl_pb_api | 3 | develop-3.0-lower | no tests | no properties
riak_sysmon | 3 | develop-3.0 | 3 EUnit tests passed, 0 failures, 59% code coverage. Points cuttlefish `rebar3` branch | No properties
riakc | 3 | develop-3.0 | 404 on https://github.com/basho/riakc ??
riak_core | 4 | develop-3.0 | | __REVISIT__ property  `bg_manager_eqc:prop_bgmgr` had masked failure, need fix (https://github.com/basho/riak_core/issues/936)
riak_api | 5 | develop-3.0 | 33 unit tests passed (tests create a `log` directory, configure needed) | no properties
riak_control | 5 | develop-3.0 | Building packages.idx multiple times... takes ages. Rebar3 build seems terribly broken (recursively depending on itself?). __REVISIT__
riak_pipe | 5 | develop-3.0 | 8 EUnit test passed, 0 failures, 5% coverage. | 1 property passed. __REVISIT__ properties not executed and PULSE based on gen_fsm, but not instrumented for gen_fsm_compat.
riak_kv | 6 | develop-3.0 --> __develop-3.0-lower__ now exists as well also __develop-3.0-merge29__ which contains develop-2.9| eqc/xref/dialyzer need work. Test data issue: https://github.com/basho/riak_kv/issues/1685 | 22 properties pass, 1 property fails __REVISIT__ riak_kv_vnode_status_mgr:prop_any_file_status()
riak_repl | 7 | __develop-3.0-lower__ | Needs to point to develop.3.0-lower in riak_kv to make it compile. Export all in test and erlang:now usage. __REVISIT__ 5 failing tests of 72, 10 canceled | Some properties pass, some fail. Need further investigation
riak_search | 7 | - | can be removed?
yokozuna | 7 | develop-3.0 | currently not integrated
riak | 8 |  develop-3.0 |

### Progress with develop-2.9 merge

For each repo used by riak which has a develop-2.9 branches (i.e. with changes from develop-2.2), there is also a need to merge develop-2.9 with develop-3.0.  For these repos this merged result will be in a `develop-3.0-merge29` branch.  Status of this work by repo is:

Repo | Status | Pull Request
:-------------------------|:-------------------------|:--------------------
riak_kv | Compiles, and All eunit and eqc tests pass (`./rebar3 as eqc eunit` and `./rebar3 eqc`).  Almost all tests correctly write to disk in `_build/test`.  Multiple issues with dialyzer | merged
riak_core | Not yet passing all tests | merged - quviq to look at outstanding failures
webmachine | Built from the `develop-2.9` branch.  All eunit tests pass.  Multiple issues with dialyzer | [PR Outstanding](https://github.com/basho/webmachine/pull/15)
mochiweb | Built from the `develop-2.9` branch.  All eunit tests pass.  Multiple issues with dialyzer | [PR Outstanding](https://github.com/basho/mochiweb/pull/28)

## Workshop 3 - late June

Date to be proposed


## Longer Term OTP-Tracking Work

a. Complete removal of deprecated gen_fsm.

b. OTP 20 migration for Riak CS

c. Logger/lager approach
