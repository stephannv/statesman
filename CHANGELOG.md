# Changelog
<!-- markdownlint-disable no-duplicate-heading -->

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## v12.1.0 5th January 2024

### Fixed

- Fixed autoloading the VERSION constants
- Fixed Ensuring inheritance issues with STI tabled
- Enabled gaplock protection when using trilogy mysql adapter

### Added

- Added Ruby 3.3 to build matrix
- Added optional initial transition

## v12.0.0 30th November 2023

### Added

- Added multi-database support [#522](https://github.com/gocardless/statesman/pull/522)
  - This now uses the correct ActiveRecord connection for the model or transition in a multi-database environment

## v11.0.0 3rd November 2023

### Changed

- Updated to support ActiveRecord > 7.2
- Remove support for:
  - Ruby; 2.7
  - Postgres; 9.6, 10, 11
  - MySQL; 5.7

## v10.2.3 2nd Aug 2023

### Fixed

- Fixed calls to reloading internal cache is the state_machine was made private / protected

## v10.2.2 21st April 2023

### Changed

- Calling `active_record.reload` resets the adapater's internal cache

## v10.2.1 3rd April 2023

### Fixed

- Fixed an edge case where `adapter.reset` were failing if the cache is empty

## v10.2.0 3rd April 2023

### Fixed

- Fixed caching of `last_transition` [#505](https://github.com/gocardless/statesman/pull/505)

## v10.1.0 10th March 2023

### Changed

- Add the source location of the guard callback to `Statesman::GuardFailedError`

## v10.0.0 17th May 2022

### Added

- Added support for Ruby 3.1 [#462](https://github.com/gocardless/statesman/pull/462)
- Added `remove_state` and `remove_transitions` methods to `Statesman::Machine` [#464](https://github.com/gocardless/statesman/pull/464)

### Changed

- Removed support for Ruby 2.5 and 2.6 [#462](https://github.com/gocardless/statesman/pull/462)

## v9.0.1 4th February 2021

### Changed

- Deprecate `ActiveRecord::Base.default_timezone` in favour of `ActiveRecord.default_timezone` [#446](https://github.com/gocardless/statesman/pull/446)

## v9.0.0 9th August 2021

### Added

- Added Ruby 3.0 support

### Breaking changes

- Removed Ruby 2.4

## v8.0.3 8th June 2021

### Added

- Implement `Machine#last_transition_to`, to find the last transition to a given state
  [#438](https://github.com/gocardless/statesman/pull/438)

## v8.0.2 30th March 2021

### Fixed

- Fixed a bug where the `history` of a model was left in an incorrect state after a transition
  conflict [#433](https://github.com/gocardless/statesman/pull/433)

## v8.0.1 20th January 2021

### Fixed

- Fixed `no implicit conversion of nil into String` error when quoting null values
  [#427](https://github.com/gocardless/statesman/pull/427)

## v8.0.0 6th January 2021

### Added

- Use AR Arel table to type cast booleans in order to avoid deprecation warning [#421](https://github.com/gocardless/statesman/pull/421)
- Support relationships that doesn't use `id` as a Primary Key
  [#422](https://github.com/gocardless/statesman/pull/422)

## v7.4.1 11th November 2020

### Added

- Add #reset method to state machine and adapter interfaces
  [#417](https://github.com/gocardless/statesman/pull/417)

## v7.4.0 26th August 2020

### Added

- [Gem Metadata](https://guides.rubygems.org/specification-reference/#metadata)
  to make finding changes between releases even easier.

## v7.3.0, 24th August 2020

### Changed

- Use correct Arel for null [#409](https://github.com/gocardless/statesman/pull/409)

## v7.2.0, 19th May 2020

### Changed

- Set non-empty password for postgres tests [#398](https://github.com/gocardless/statesman/pull/398)
- Handle transitions differently for MySQL [#399](https://github.com/gocardless/statesman/pull/399)
- pg requirement from >= 0.18, <= 1.1 to >= 0.18, <= 1.3 [#400](https://github.com/gocardless/statesman/pull/400)
- Lazily enable mysql gaplock protection [#402](https://github.com/gocardless/statesman/pull/402)

## v7.1.0, 10th Feb 2020

### Fixed

- Fix `to_s` on `TransitionFailedError` & `GuardFailedError`. `.message` and
    `.to_s` diverged when `from` and `to` accessors where added in v4.1.3

## v7.0.1, 8th Jan 2020

### Fixed

- Fix deprecation warning with Ruby 2.7 [#386](https://github.com/gocardless/statesman/pull/386)

## v7.0.0, 8th Jan 2020

### Breaking changes

- Drop official support for Rails 4.2, 5.0 and 5.1, following our [compatibility
  policy](https://github.com/gocardless/statesman/blob/master/docs/COMPATIBILITY.md).

## v6.0.0, 20th December 2019

### Breaking changes

- Drop official support for Ruby 2.2 and 2.3 following our [compatibility
  policy](https://github.com/gocardless/statesman/blob/master/docs/COMPATIBILITY.md).

## v5.2.0, 17th December 2019

### Changed

- Issue `most_recent_transition_join` query as a single-line string [#381](https://github.com/gocardless/statesman/pull/381)

## v5.1.0, 22th November 2019

### Fixed

- Correct `Statesman::Adapters::ActiveRecordQueries` error text [@Bramjetten](https://github.com/gocardless/statesman/pull/376)
- Removes duplicate `map` call [Isaac Seymour](https://github.com/gocardless/statesman/pull/362)

### Changed

- Update changelog with instructions of how to use `ActiveRecordQueries` added
  in v5.0.0
- Pass exception into `after_transition_failure` and `after_guard_failure` callbacks [@credric-cordenier](https://github.com/gocardless/statesman/pull/378)

## v5.0.0, 11th November 2019

### Added

- Adds new syntax and restrictions to ActiveRecordQueries [PR#358](https://github.com/gocardless/statesman/pull/358). With the introduction of this, defining `self.transition_class` or `self.initial_state` is deprecated and will be removed in the next major release.
  Change

  ```ruby
    include Statesman::Adapters::ActiveRecordQueries
    def self.initial_state
      :initial
    end
    def self.transition_class
      MyTransition
    end
  ```

  to

  ```ruby
    include Statesman::Adapters::ActiveRecordQueries[
      initial_state: :initial,
      transition_class: MyTransition
    ]
  ```

## v4.1.4, 11th November 2019

### Changed

- Reverts the breaking changes from [PR#358](https://github.com/gocardless/statesman/pull/358) & `v4.1.3` that where included in the last minor release. If you have changed your code to work with these changes `v5.0.0` will be a copy of `v4.1.3` with a bugfix applied.

## v4.1.3, 6th November 2019

### Added

- Add accessible from / to state attributes on the `TransitionFailedError` to avoid parsing strings [@ahjmorton](https://github.com/gocardless/statesman/pull/367)
- Add `after_transition_failure` mechanism [@credric-cordenier](https://github.com/gocardless/statesman/pull/366)

## v4.1.2, 17th August 2019

### Added

- Add support for Rails 6 [@greysteil](https://github.com/gocardless/statesman/pull/360)

## v4.1.1, 6th July 2019

### Fixed

- Fix statesman index detection for indexes that start t-z [@hmarr](https://github.com/gocardless/statesman/pull/354)
- Correct access of metadata via `state_machine` [@glenpike](https://github.com/gocardless/statesman/pull/349)

## v4.1.0, 10 April 2019

### Changed

- Add better support for mysql (and others) in `transition_conflict_error?` [@greysteil](https://github.com/greysteil) (<https://github.com/gocardless/statesman/pull/342>)

## v4.0.0, 22 February 2019

### Fixed

- Fixes an issue with `after_commit` transition blocks that where being
    executed even if the transaction rolled back. ([patch](https://github.com/gocardless/statesman/pull/338) by [@matid](https://github.com/matid))

### Changed

- Forces Statesman to use a new transactions with `requires_new: true` (<https://github.com/gocardless/statesman/pull/249>)

## v3.5.0, 2 November 2018

### Changed

- Expose `most_recent_transition_join` - ActiveRecords `or` requires that both
    sides of the query match up. Exposing this methods makes things easier if
    one side of the `or` uses `in_state` or `not_in_state`. (patch by [@adambutler](https://github.com/adambutler))
- Various Readme and CI related changes.

## v3.4.1, 14 February 2018 ❤️

### Added

- Support ActiveRecord transition classes which don't include `Statesman::Adapters::ActiveRecordTransition`, and thus don't have a `.updated_timestamp_column` method (see #310 for further details) (patch by [@timrogers](https://github.com/timrogers))

## v3.4.0, 12 February 2018

### Changed

- When unsetting the `most_recent` flag during a transition, don't assume that transitions have an `updated_at` attribute, but rather allow the "updated timestamp column" to be re-configured or disabled entirely (patch by [@timrogers](https://github.com/timrogers))

## v3.3.0, 5 January 2018

### Changed

- Touch `updated_at` on transitions when unsetting `most_recent` flag (patch by [@NGMarmaduke](https://github.com/NGMarmaduke))
- Fix `force_reload` for ActiveRecord models with loaded transitions (patch by [@jacobpgn](https://github.com/))

## v3.2.0, 27 November 2017

### Added

- Allow specifying metadata with `Machine#allowed_transitions` (patch by [@vvondra](https://github.com/vvondra))

## v3.1.0, 1 September 2017

### Added

- Add support for Rails 5.0.x and 5.1.x (patch by [@kenchan0130](https://github.com/kenchan0130) and [@timrogers](https://github.com/timrogers))

### Changed

- Run tests in CircleCI instead of TravisCI (patch by [@timrogers](https://github.com/timrogers))
- Update Rubocop and fix offences (patch by [@timrogers](https://github.com/timrogers))

## v3.0.0, 3 July 2017

### Breaking changes

- Drop support for Rails < 4.2
- Drop support for Ruby < 2.2

For details on our compatibility policy, see `docs/COMPATIBILITY.md`.

### Changed

- Better handling of custom transition association names (patch by [@greysteil](https://github.com/greysteil))
- Add foreign keys to transition table generator (patch by [@greysteil](https://github.com/greysteil))
- Support partial indexes in transition table update generator (patch by [@kenchan0130](https://github.com/kenchan0130))

## v2.0.1, 29 March 2016

### Added

- Add support for Rails 5 (excluding Mongoid adapter)

## v2.0.0, 5 January 2016

- No changes from v2.0.0.rc1

## v2.0.0.rc1, 23 December 2015

### Breaking changes

- Unset most_recent after before transitions
  - TL;DR: set `autosave: false` on the `has_many` association between your parent and transition model and this change will almost certainly not affect your integration
  - Previously the `most_recent` flag would be set to `false` on all transitions during any `before_transition` callbacks
  - After this change, the `most_recent` flag will still be `true` for the previous transition during these callbacks
  - Whilst this behaviour is almost certainly what your integration already expected, as a result of it any attempt to save the new, as yet unpersisted, transition during a `before_transition` callback will result in a uniqueness error. In particular, if you have not set `autosave: false` on the `has_many` association between your parent and transition model then any attempt to save the parent model during a `before_transition` will result in an error
- Require a most_recent column on transition tables
  - The `most_recent` column, added in v1.2.0, is now required on all transition tables
  - This greatly speeds up queries on large tables
  - A zero-downtime migration path is outlined in the changelog for v1.2.0. You should use that migration path **before** upgrading to v2.0.0
- Increase default initial sort key to 10
- Drop support for Ruby 1.9.3, which reached end-of-life in February 2015
- Move support for events to a companion gem
  - Previously, Statesman supported the use of "events" to trigger transitions
  - To keep Statesman lightweight we've moved event functionality into the `statesman-events` gem
  - If you are using events, add `statesman-events` to your gemfile and include `Statesman::Events` in your state machines

### Changed

- Add after_destroy hook to ActiveRecord transition model templates
- Add `in_state?` instance method to `Statesman::Machine`
- Add `force_reload` option to `Statesman::Machine#last_transition`

## v1.3.1, 2 July 2015

### Changed

- Fix `in_state` queries with a custom `transition_name` (patch by [0tsuki](https://github.com/0tsuki))
- Fix `backfill_most_recent` rake task for databases that support partial indexes (patch by [greysteil](https://github.com/greysteil))

## v1.3.0, 20 June 2015

### Changed

- Rename `last_transition` alias in `ActiveRecordQueries` to `most_recent_#{model_name}`, to allow merging of two such queries (patch by [@isaacseymour](https://github.com/isaacseymour))

## v1.2.5, 17 June 2015

### Changed

- Make `backfill_most_recent` rake task db-agnostic (patch by [@timothyp](https://github.com/timothyp))

## v1.2.4, 16 June 2015

### Changed

- Clarify error messages when misusing `Statesman::Adapters::ActiveRecordTransition` (patch by [@isaacseymour](https://github.com/isaacseymour))

## v1.2.3, 14 April 2015

### Changed

- Fix use of most_recent column in MySQL (partial indexes aren't supported) (patch by [@greysteil](https://github.com/greysteil))

## v1.2.2, 24 March 2015

### Added

- Add support for namespaced transition models (patch by [@DanielWright](https://github.com/DanielWright))

## v1.2.1, 24 March 2015

### Added

- Add support for Postgres 9.4's `jsonb` column type (patch by [@isaacseymour](https://github.com/isaacseymour))

## v1.2.0, 18 March 2015

### Added

- Add a `most_recent` column to transition tables to greatly speed up queries (ActiveRecord adapter only).
  - All queries are backwards-compatible, so everything still works without the new column.
  - The upgrade path is:
    - Generate and run a migration for adding the column, by running `rails generate statesman:add_most_recent <ParentModel> <TransitionModel>`.
    - Backfill the `most_recent` column on old records by running `rake statesman:backfill_most_recent[ParentModel]`.
    - Add constraints and indexes to the transition table that make use of the new field, by running `rails g statesman:add_constraints_to_most_recent <ParentModel> <TransitionModel>`.
  - The upgrade path has been designed to be zero-downtime, even on large tables. As a result, please note that queries will only use the `most_recent` field after the constraints have been added.

### Changed

- `ActiveRecordQueries.{not_,}in_state` now accepts an array of states.

## v1.1.0, 9 December 2014

### Fixed

- Support for Rails 4.2.0.rc2:
  - Remove use of serialized_attributes when using 4.2+. (patch by [@greysteil](https://github.com/greysteil))
  - Use reflect_on_association rather than directly using the reflections hash. (patch by [@timrogers](https://github.com/timrogers))
- Fix `ActiveRecordQueries.in_state` when `Model.initial_state` is defined as a symbol. (patch by [@isaacseymour](https://github.com/isaacseymour))

### Changed

- Transition metadata now defaults to `{}` rather than `nil`. (patch by [@greysteil](https://github.com/greysteil))

## v1.0.0, 21 November 2014

No changes from v1.0.0.beta2

## v1.0.0.beta2, 10 October 2014

### Breaking changes

- Rename `ActiveRecordModel` to `ActiveRecordQueries`, to reflect the fact that it mixes in some helpful scopes, but is not required.

## v1.0.0.beta1, 9 October 2014

### Breaking changes

- Classes which include `ActiveRecordModel` must define an `initial_state` class method.

### Fixed

- `ActiveRecordModel.in_state` and `ActiveRecordModel.not_in_state` now handle inital states correctly (patch by [@isaacseymour](https://github.com/isaacseymour))

### Added

- Transition tables created by generated migrations have `NOT NULL` constraints on `to_state`, `sort_key` and foreign key columns (patch by [@greysteil](https://github.com/greysteil))
- `before_transition` and `after_transition` allow an array of to states (patch by [@isaacseymour](https://github.com/isaacseymour))

## v0.8.3, 2 September 2014

### Fixed

- Optimisation for Machine#available_events (patch by [@pacso](https://github.com/pacso))

## v0.8.2, 2 September 2014

### Fixed

- Stop generating a default value for the metadata column if using MySQL.

## v0.8.1, 19 August 2014

### Fixed

- Adds check in Machine#transition to make sure the 'to' state is not an empty array (patch by [@barisbalic](https://github.com/barisbalic))

## v0.8.0, 29 June 2014

### Added

- Events. Machines can now define events as a logical grouping of transitions (patch by [@iurimatias](https://github.com/iurimatias))
- Retries. Individual transitions can be executed with a retry policy by wrapping the method call in a `Machine.retry_conflicts {}` block (patch by [@greysteil](https://github.com/greysteil))

## v0.7.0, 25 June 2014

### Added

- `Adapters::ActiveRecord` now handles `ActiveRecord::RecordNotUnique` errors explicitly and re-raises with a `Statesman::TransitionConflictError` if it is due to duplicate sort_keys (patch by [@greysteil](https://github.com/greysteil))

## v0.6.1, 21 May 2014

### Fixed

- Fixes an issue where the wrong transition was passed to after_transition callbacks for the second and subsequent transition of a given state machine (patch by [@alan](https://github.com/alan))

## v0.6.0, 19 May 2014

### Added

- Generators now handle namespaced classes (patch by [@hrmrebecca](https://github.com/hrmrebecca))

### Changed

- `Machine#transition_to` now only swallows Statesman generated errors. An exception in your guard or callback will no longer be caught by Statesman (patch by [@paulspringett](https://github.com/paulspringett))

## v0.5.0, 27 March 2014

### Added

- Scope methods. Adds a module which can be mixed in to an ActiveRecord model to provide `.in_state` and `.not_in_state` query scopes.
- Adds `Machine#after_initialize` hook (patch by [@att14](https://github.com/att14))

### Fixed

- Added MongoidTransition to the autoload statements, fixing [#29](https://github.com/gocardless/statesman/issues/29) (patch by [@tomclose](https://github.com/tomclose))

## v0.4.0, 27 February 2014

### Added

- Adds after_commit flag to after_transition for callbacks to be executed after the transaction has been
committed on the ActiveRecord adapter. These callbacks will still be executed on non transactional adapters.

## v0.3.0, 20 February 2014

### Added

- Adds Machine#allowed_transitions method (patch by [@prikha](https://github.com/prikha))

## v0.2.1, 31 December 2013

### Fixed

- Don't add attr_accessible to generated transition model if running in Rails 4

## v0.2.0, 16 December 2013

### Added

- Adds Ruby 1.9.3 support (patch by [@jakehow](https://github.com/jakehow))
- All Mongo dependent tests are tagged so they can be excluded from test runs

### Changed

- Specs now crash immediately if Mongo is not running

## v0.1.0, 5 November 2013

### Added

- Adds Mongoid adapter and generators (patch by [@dluxemburg](https://github.com/dluxemburg))

### Changed

- Replaces `config#transition_class` with `Statesman::Adapters::ActiveRecordTransition` mixin. (inspired by [@cjbell88](https://github.com/cjbell88))
- Renames the active record transition generator from `statesman:transition` to `statesman:active_record_transition`.
- Moves to using `require_relative` internally where possible to avoid stomping on application load paths.

## v0.0.1, 28 October 2013

- Initial release
