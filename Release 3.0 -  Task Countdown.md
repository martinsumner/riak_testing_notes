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
- plus other key individuals - e.g. Christopher Meiklejohn, Antonio Nikshiev.

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

incomplete ... sample issues only ...

Test | Status
:-------------------------|:-------------------------|
Sidejob eqc tests | Was already a known issue, but now  more often failing in OTP20
Poolboy eqc tests | eqc tests do pass on upstream devinus branch


## Workshop 2 - late April / early May

tba

## Longer Term OTP-Tracking Work

a. Complete removal of deprecated gen_fsm.

b. OTP 20 migration for Riak CS

c. Logger/lager approach
