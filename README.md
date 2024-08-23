# bashfactor

Hi, my name is Bashlund and I am a termie-holic.

Enough about me, let's talk about shell scripting and `bashfactor` !

Using `bashfactor` you can refactor source code straight from the terminal!

`bashfactor` was made originally to refactor TypeScript code from the terminal using bash scripting, but with minimal twiddling you should be able to refacator any compiled language using `bashfactor`.

## How does bashfactor work

The secret sauce is that `bashfactor` captures the error output from the compiler and then runs `sed` to substitute `old` to `new` in the files where the compiler flagged errors.

Beautiful? Hacky as hell? Yes mam! That is why we love shell scripting! Beautiful hacks unite.

Is this really written in Bash?

No, no, this tool is written in the more primitive POSIX compliant shell dialect which runs under bash, dash and BusyBox shell.

## How to use bashfactor

Clone the repo or just copy the file `bashfactor` and put in in your `$PATH` or wherever you want, and make sure the file is executable (`chmod +x bashfactor`).

1. First make sure your project compiles without errors.
2. Then, introduce an error by renaming the target variable/function/class where it is declared, this will result in compiler errors.
3. Now, run bashfactor to resolve those errors by renaming all other error occurrences for you.

```sh
bashfactor -i oldname newname
```

The `-i` flag is optional and brings up your editor to inspect and/or modify the list of files to substitute in.

By default `bashfactor` runs `npx tsc` to compile. This you can change as you wish either in the source file in the `COMPILE` function or you can override the setting using the `BASHFACTOR_CMD` env variable, as:  

```sh
BASHFACTOR_CMD="npm run build" bashfactor -i oldname newname
```

If you happen to run your `node` and `npm` inside a container for isolation you can do this:  

```sh
BASHFACTOR_CMD="docker exec -i mycontainer npx tsc" bashfactor -i oldname newname
```

Note that if running in a container and your compiler yields absolute filepaths instead of relative paths the substitution will not work. The easiest solution to this is to just run the `bashfactor` script from a shell inside the container.

If you switch to another language and compiler that error output might be in a different format and in such case you need to adjust the `PARSE` function in the script.

Bashfactor will run further substitution rounds automatically since other compilation error surface as some are fixed.

### Changing editor

The default editor `bashfactor` fires up is `$EDITOR`, then `vim`. You can set this as:  

```sh
EDITOR=nano bashfactor -i oldname newname
```

## License

bashfactor (C) @bashlund 2024 MIT license.
