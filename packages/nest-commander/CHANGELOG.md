# nest-commander

## 3.6.0

### Minor Changes

- 7f54ff8: Add serviceErrorHandler option

  This option allows for catching and handling errors at the Nest service execution level so that
  lifecycle hooks still properly work. By default it is set to
  `(err: Error) => process.stderr.write(err.toString())`.

- 09b6134: Add the ability to have a Root command

  With the `@RootCommand()` the `-h` flag can now output the options of the default command _along
  with_ the names of the other commands.

## 3.5.0

### Minor Changes

- d2e5fc8: Allow for use of request scoped providers through a new module decorator

  By making use of the `@RequestModule()` decorator for modules, as mock request object can be set
  as a singleton to help the use of `REQUEST` scoped providers in a singleton context. There's now
  also an error that is logged in the case of a property of `undefined` being called, as this is
  usually indicative of a `REQUEST` scoped provider being called from a `SINGLETON` context.

## 3.4.0

### Minor Changes

- fadb70d: Allow for a sub command to be set as the default sub command.
- 74c88f5: Add new api registerWithSubCommand to CommandRunner Class
- abff78d: Allow for options to be parsed positionally via an option passed to CommandFactory

## 3.3.0

### Minor Changes

- 8c639d3: fix: update module resolution to node16 so dynamic imports are not transpiled out during
  TS build

## 3.2.1

### Patch Changes

- 5c089a6: Fixed an issue preventing use of ESM packages as plugins in the command factory

## 3.2.0

### Minor Changes

- 84e6b95: Add `env` option to `@Option()` decorator

## 3.1.0

### Minor Changes

- 15da048: InquirerService now exposes inquirer publicly

## 3.0.0

### Major Changes

- d6ebe0e: Migrate `CommandRunner` from interface to abstract class and add `.command`

  This change was made so that devs could access `this.command` inside the `CommandRunner` instance
  and have access to the base command object from commander. This allows for access to the `help`
  commands in a programatic fashion.

  To update to this version, any `implements CommandRunner` should be changed to
  `extends CommandRunner`. If there is a `constructor` to the `CommandRunner` then it should also
  use `super()`.

### Minor Changes

- 3d2aa9e: Update NestJS package to version 9
- a8d109f: Upgrade commander to v9.4.0

### Patch Changes

- c30a4de: Ensure the parser for choices is always called

## 2.5.0

### Minor Changes

- 2d8a143: Added support for aliased subcommands
- 6e39331: Allow for command options to have defined choices

  Option choices are now supported either as a static string array or via the `@OptionChoicesFor()`
  decorator on a class method. This decorator method approach allows for using a class's injected
  providers to give the chocies, which means they could come from a database or a config file
  somewhere if the CLI is set up to handle such a case

## 2.4.0

### Minor Changes

- eaa63fb: Adds a new CliUtilityService and @InjectCommander() decorator

  There is a new `CliUtilityService` and `@InjectCommander()` decorator that allows for direct
  access to the commander instance. The utility service has methods like `parseBoolean`, `parseInt`,
  and `parseFloat`. The number parsing methods are just simple wrappers around `Number.parse*()`,
  but the boolean parsing method handles true being `yes`, `y`, `1`, `true`, and `t` and false being
  `no`, `n`, `false`, `f`, and `0`.

## 2.3.5

### Patch Changes

- 55eb46d: Update peerDependencies for nest-commander to include @types/inquirer

## 2.3.4

### Patch Changes

- 3ad2c3a: Add cosmiconfig to the dependencies for proper publishing

## 2.3.3

### Patch Changes

- 8285a98: missing config file warning doesn't show

## 2.3.2

### Patch Changes

- 3c43005: fix: move plugin error code to onModuleInit

## 2.3.1

### Patch Changes

- 478c0d9: Make commands built with `usePlugins: true` not exit on non-found config file, just log
  extra data when an error happens

## 2.3.0

### Minor Changes

- 6c9eaa3: Commands can now be built with the expectation of reading in plugins to dynamically
  modify the CLI

  By using the `usePlugins` option for the `CommandFactory`, the built CLI can expect to find a
  configuration file at `nest-commander.json` (or several others, check the docs) to allow for users
  to plug commands in after the CLI is built.

- 13723bd: Subcommands can now be created

  There's a new decorator, `@SubCommand()` for creating nested commands like `docker compose up`.
  There's also a new option on `@Command()` (`subCommands`) for setting up this sub command
  relationship.

## 2.2.0

### Minor Changes

- 3831e52: Adds a new `@Help()` decorator for custom commander help output

  `nest-commander-testing` now also uses a `hex` instead of `utf-8` encoding when creating a random
  js file name during the `CommandTestFactory` command. This is to help create more predictable
  output names.

## 2.1.0

### Minor Changes

- 6df8964: Adds in a new metadata option for the @Option() decorator to make the option required,
  just like a required argument

## 2.0.0

### Major Changes

- ee001cc: Upgrade all Nest dependencies to version 8

  WHAT: Upgrade `@nestjs/` dependencies to v8 and RxJS to v7 WHY: To support the latest version of
  Nest HOW: upgrading to Nest v8 should be all that's necessary (along with rxjs to v7)

## 1.3.0

### Minor Changes

- f3f687b: Allow for commands to be run indefinitely

  There is a new `runWithoutClosing` method in the `CommandFactory` class. This command allows for
  not having the created Nest Application get closed immediately, which should allow for the use of
  indefinitely runnable commands.

## 1.2.0

### Minor Changes

- 7cce284: Add ability to use error handler for commander errors

  Within the `CommandFactory.run()` now as a second parameter you can either keep passing just the
  logger, or you can pass in an object with the logger and an `errorHandler`. Ths `errorHandler` is
  a method that takes in an `Error` and returns `void`. The errorHandler will be passed to
  commander's `exitOverride` method, if it exists. This is useful for better handling errors and
  giving the dev more control over what is seen. There is also no longer an
  `unhandledPromiseRejection` on empty commands.
