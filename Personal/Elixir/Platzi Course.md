### 2 - ¿Por qué Elixir?
Es una alternativa a GoLang, pero **más productiva**.  
A nivel de concurrencia (que es por lo que brilla GoLang) Elixir es muy poderoso y esto es gracias a **Erlang** y el **Framework OTP**.  
Erlang es **concurrente** y **tolerante a fallas**. Tiene un modelo de procesos ligeros que facilita la creación de sistemas que escalan a millones de conexiones con estado. Así logró crecer whatsapp .  
OTP trae muchas herramientas y entre ellas el **actor model** de Erlang, que está basado en procesos ligeros que hace a elixir eficiente.

Adicionalmente Elixir agrega features (lisp-style) como los macros y protocolos.** Inmutabilidad** y no tiene **side effects** que hacen al sistema mucho más facil de entender y debuggear.  
Elixir es la **versión Práctica Industrial** de Haskell (que es mucho más académico)

[Elixir vs Go](https://www.cogini.com/blog/advantages-of-elixir-vs-golang/)  
[Elixir & OTP](https://serokell.io/blog/elixir-otp-guide)

### 3 - Primeros pasos
```shell
asdf list-all erlang
2# ...
323.2
423.2.1
523.2.2
6# ...
```

```shell
asdf install erlang 23.2.1
```

```shell
asdf list-all elixir
2# ...
31.11.2
41.11.2-otp-21
51.11.2-otp-22
61.11.2-otp-23
7# ...
```

```shell
asdf install elixir 1.11.2-otp-23
```

#### Set Versions

Now that you have Elixir and Erlang/OTP installed, you can save your chosen versions in a project. From the root of the project, run:

```shell
1asdf local erlang 23.2.1
2asdf local elixir 1.11.2-otp-23
```

shell

Replace the versions above with those you used during installation. This will create a `.tool-versions` file in your project, which will instruct ASDF which versions to use. If you'd like to set a global, or default, version, run:

```shell
1asdf global erlang 23.2.1
2asdf global elixir 1.11.2-otp-23
```

shell

This will create a `.tool-versions` file in your home directory. ASDF will use these versions whenever a project doesn't specify versions of its own.

#### Conclusion

ASDF is a popular version manager for Elixir projects because it manages both Elixir and Erlang versions. Include a `.tool-versions` file in your projects to help contributors get up-and-running quickly, and don't forget to leverage the `-otp-XY` versions of Elixir for the best compatibility.

If you'd like to learn more about Elixir and what it can do for your next project, check out [Elixir: The Big Picture](https://app.pluralsight.com/library/courses/elixir-big-picture/) here on Pluralsight.

### 13 Functions & Modules

defmodule -> modules
def -> function
defp -> private functions
fn -> anonymous function

## 18 - IO, Path, File

https://hexdocs.pm/elixir/IO.html

https://hexdocs.pm/elixir/Path.html

https://hexdocs.pm/elixir/File.html

## 21 - Module attributes
https://hexdocs.pm/elixir/Module.html

@moduledoc - document module
@doc -> document functions