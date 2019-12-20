### Install

You can then easily install all of the dependencies with:

```bash
brew bundle
```

If a dependency is already installed and there is an upgrade available it will be upgraded.

`brew bundle` will look for `Brewfile` in the current directory. Use `--file` to specify a path to a different `Brewfile`, or set the `HOMEBREW_BUNDLE_FILE` environment variable, with `--file` taking precedence if both are provided.

You can skip the installation of dependencies by adding space-separated values to one or more of the following environment variables:

- `HOMEBREW_BUNDLE_BREW_SKIP`
- `HOMEBREW_BUNDLE_CASK_SKIP`
- `HOMEBREW_BUNDLE_MAS_SKIP`
- `HOMEBREW_BUNDLE_TAP_SKIP`

`brew bundle` will output a `Brewfile.lock.json` in the same directory as the `Brewfile` if all dependencies are installed successfully. This contains dependency and system status information which can be useful in debugging `brew bundle` failures and replicating a "last known good build" state.

You can opt-out of this behaviour by setting the `HOMEBREW_BUNDLE_NO_LOCK` environment variable or passing the `--no-lock option`.

You may wish to check this file into the same version control system as your `Brewfile` (or ensure your version control system ignores it if you'd prefer to rely on debugging information from a local machine).

### Dump

You can create a `Brewfile` from all the existing Homebrew packages you have installed with:

```bash
brew bundle dump
```

The `--force` option will allow an existing `Brewfile` to be overwritten as well.
The `--describe` option will output a description comment above each line.
The `--no-restart` option will prevent `restart_service` from being added to ``brew`` lines with running services.

### Cleanup

You can also use `Brewfile` to list the only packages that should be installed, removing any package not present or dependent. This workflow is useful for maintainers or testers who regularly install lots of formulae. To uninstall all Homebrew formulae not listed in `Brewfile`:

```bash
brew bundle cleanup
```

Unless the `--force` option is passed, formulae that would be uninstalled will be listed rather than actually be uninstalled.

### Check

You can check there's anything to install/upgrade in the `Brewfile` by running:

```bash
brew bundle check
```

This provides a successful exit code if everything is up-to-date so is useful for scripting.

For a list of dependencies that are missing, pass `--verbose`. This will also check _all_ dependencies by not exiting on the first missing dependency category.

### List

Outputs a list of all of the entries in the Brewfile.

```bash
brew bundle list
```

Pass one of `--casks`, `--taps`, `--mas`, or `--brews` to limit output to that type. Defaults to `--brews`. Pass `--all` to see everything.

Note that the _type_ of the package is **not** included in this output.

### Exec

Runs an external command within Homebrew's superenv build environment:

```bash
brew bundle exec -- bundle install
```

This sanitized build environment ignores unrequested dependencies, which makes sure that things you didn't specify in your `Brewfile` won't get picked up by commands like `bundle install`, `npm install`, etc. It will also add compiler flags which will help find keg-only dependencies like `openssl`, `icu4c`, etc.

### Restarting services

You can choose whether `brew bundle` restarts a service every time it's run, or
only when the formula is installed or upgraded in your `Brewfile`:

```ruby
# Always restart myservice
brew 'myservice', restart_service: true

# Only restart when installing or upgrading myservice
brew 'myservice', restart_service: :changed
```