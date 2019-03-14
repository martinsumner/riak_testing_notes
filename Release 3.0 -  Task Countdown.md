# Release 3.0

Release 3.0 has two primary goals:

- Move forward to be compatible with OTP 20 (without backwards compatibility);
- Look to deprecate features to simplify the long-term structure of Riak.

The aim is to provide confidence to current Riak customers that Riak will be efficiently maintainable for at least the next o(10) years, but also make Riak easier to adopt within existing scale-out Erlang/Elixir environments.

Release 3.0 will not support a smooth upgrade from multiple previous Riak versions.  It will be tested only for migration using node join/leave events from Riak 2.9.

## Background to the work on 3.0

The work on release 3.0 builds on the efforts of multiple people who have contributed to OTP20 on Riak before, in particular Ted Burghart and Andrew Thompson.  The release is now being worked on by a joint team, through a series of workshop, pulling in experience from:

- Quviq;
- NHS;
- Airelogic;
- bet365;
- plus other key individuals - e.g. Christopher Meiklejohn, Antonio Nikishaev.

## Workshop 1 - 6th March to 8th March

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




### Specific Challenges prior to Next Workshop

Task | Status
:-------------------------|:-------------------------:|
Write-up simplified instructions for deploying and running riak_test | assigned - martin s
Continue work on unit/eqc test pass rates | to be assigned
First merge in of develop-2.9 | assigned - martin s
First pass at running riak_test, analysis of failures and reasons (needs `make devrel`) | unassigned
First pass at dialyzer, analysis of failures and reasons | unassigned
Discuss with andrew develop-3.0 vs develop-3.0-lower | assigned - hans
Write a shot developer guide for working on 3.0 | unassigned

### Known test issues


REPO | NODE | NEW_BRANCH | NOTES
-- | -- | -- | --
basho_stats | 0  | develop-3.0 | neither EUnit tests nor QC tests run because missing_R_executable file
bear | 0 | develop-3.0 | use upstream
canola | 0 | develop-3.0 |
cluster_info | 0 | develop-3.0 |
ebloom | 0 | develop-3.0 |
edown | 0 | develop-3.0 |
eleveldb | 0  | develop-3.0 |
eper | 0 | develop-3.0 | use upstream? 83 EUnit tests passed, 0 failures, 51% coverage. No ECQ tests.
erlang_js | 0 | develop-3.0 | removed?
erlydtl | 0 | develop-3.0 |
eunit_formatters | 0 | develop-3.0 | use upstream
fuse | 0 | develop-3.0 |
getopt | 0 | develop-3.0 |
goldrush | 0 | develop-3.0 | use upstream
hamcrest | 0 | develop-3.0 |
ibrowse | 0 | develop-3.0 |
kvc | 0 | develop-3.0 | use upstream
meck | 0 | develop-3.0 |
mochiweb | 0 | develop-3.0 | need to consider upstream
neotama | 0 | develop-3.0 |
node_package | 0 | develop-3.0 |
pbkdf2 | 0 | develop-3.0 | 46 EUnit test passed, 0 failures, 
poolboy | 0 | develop-3.0 | merge with upstream. (devinus: all 20 EUnit tests passed, 62% coverage. 2 EQC properties passed, 71.5% coverage. kyorai: all 12 EUnit tests passed, 76% coverage. EQC did not run at first but after some fixing (not complicated) the same 2 EQC properties passed, 73% coverage)
proper | 0 | develop-3.0 |
ranch | 0 | develop-3.0 | use upstream
rebar_lock_deps_plugin | 0 | develop-3.0 | provided by rebar3
riak_dt | 0 | develop-3.0 | dialyzer needs work. All 105 EUnit tests passed, 76% coverage. 20 EQC tests passed.
sidejob | 0 | develop-3.0 | QuickCheck tests fail due to known race, appears more likely to fail in OTP20. No EUnit tests.
stdlib2 | 0 | develop-3.0 |
syslog | 0 | develop-3.0 | use upstream
folsom | 1 | develop-3.0 | use upstream
hyper | 1 | develop-3.0 | one unit test fails, dialyzer
lager | 1 | develop-3.0 | use erlang-lager fork
lager_syslog | 1 | develop-3.0 | use erlang-lager fork
parse_trans | 1 | develop-3.0 |
protobuffs | 1 | n/a | rebar_gpb_plugin now used with tsloughter/riak_pb_msgcodegen
riak_auth_mods | 1 | develop-3.0 |
setup | 1 | develop-3.0 |
sext | 1 | develop-3.0 | 24 EQC properties passed
webmachine | 1 |  develop-3.0 | use upstream - will need to add support for configurable receive buffer https://github.com/webmachine/webmachine/issues/299
cuttlefish | 2 | develop-3.0 |
exometer_core | 2 | develop-3.0 | use upstream
merge_index | 2 | - | can be removed?
riak_ensemble | 2 | develop-3.0 | dialyzer fails, copied in files from riak_test. 19 EUnit test passed, nothing about failures, 71% coverage. 1 EQC test passed
riak_pb | 2 | develop-3.0-lower | dialyzer isn't clean & I had to make a rebar3 plugin that's currently hosted under my own GH account
riaknostic | 2 | - | can be removed?
bitcask | 3 | develop-3.0 | depends on 'rebar3' branch of cuttlefish, dialyzer not clean
clique | 3 | develop-3.0 | depends on 'rebar3' branch of cuttlefish
riak_repl_pb_api | 3 | develop-3.0-lower |
riak_sysmon | 3 | develop-3.0 | 3 EUnit tests passed, 0 failures, 59% code coverage. No EQC tests
riakc | 3 | develop-3.0 |
riak_core | 4 | develop-3.0 |
riak_api | 5 |  develop-3.0 | gen_fsm warnings
riak_control | 5 | develop-3.0 |
riak_pipe | 5 | develop-3.0 | 8 EUnit test passed, 0 failures, 5% coverage. 1 EQC passed.
riak_kv | 6 | develop-3.0 | eqc/xref/dialyzer need work
riak_repl | 7 | develop-3.0 |
riak_search | 7 | - | can be removed?
yokozuna | 7 | develop-3.0 | currently not integrated
riak | 8 |  develop-3.0 |



## Workshop 2 - late April / early May

tba

## Longer Term OTP-Tracking Work

a. Complete removal of deprecated gen_fsm.

b. OTP 20 migration for Riak CS

c. Logger/lager approach
