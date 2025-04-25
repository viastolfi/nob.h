> [!WARNING]
> This is a fork from [https://github.com/tsoding/nob.h](https://github.com/tsoding/nob.h)
> I made some changes to it so it feats my need 
> I may or may not have break anything else
> Prefer using the main lib to avoid unplanned behavior

### Changes I've done 

add the `nob_find_source_in_dir_recursively(Nob_Cmd *cmd, const char* path)` function to add all your source files to your cmd from a specific dir.

### Example

```
src
|
|- main.c
|- foo.c
- bar/
  |
  |- bar.c
```

Using the `nob_find_source_in_dir_recursively` funtion, you can add directly all .c files to your cmd using it like that

```c
int main(int argc, char** argv)
{
    NOB_GO_REBUILD_URSELF(argc, argv);

    if (!nob_mkdir_if_not_exists("build/")) return 1;

    Nob_Cmd cmd = {0};

    nob_cmd_append(&cmd, "gcc", "-Wall", "-Wextra", "-g", "-o", "build/example");

    Nob_String_Builder sources = {0};
    if(!nob_find_source_in_dir_recursively(&cmd, "src/"))
    {
        nob_log(NOB_ERROR, "could not nob_find_source_in_dir_recursively with %s", "src/");
        return 1;
    }
    
    if (!nob_cmd_run_sync_and_reset(&cmd)) return 1;

    return 0;
}
```

# nob.h - Next generation of the NoBuild

This library is the next generation of the NoBuild idea. "nob" stands for "nobuild", but it's shorter and more suitable as a prefix for a library.

For my previous iteration on this idea see [https://github.com/tsoding/nobuild](https://github.com/tsoding/nobuild), but I do not recommend to use it.

## NoBuild Idea

The idea is that you should not need anything but a C compiler to build a C project. No make, no cmake, no shell, no cmd, no PowerShell etc. Only C compiler. So with the C compiler you bootstrap your build system and then you use the build system to build everything else.

## This is an Experimental Project

I'm not sure if this is even a good idea myself. This is why I'm implementing it. This is a research project. I'm not making any claims about suitability of this approach to any project.

Right now I'm actively using nob in a variety of my C projects at [https://github.com/tsoding/](https://github.com/tsoding/). It works quite well for me there.

## It's likely Not Suitable for Your Project

If you are using [cmake](https://cmake.org/) with tons of modules to manage and find tons of dependencies you probably don't want to use this tool. (But in that case I personally think you have much bigger problem than a build system). NoBuild is more like writting shell scripts but in C.

## Advantages of NoBuild

- Extremely portable builds across variety of systems including (but not limited to) Linux, MacOS, Windows, FreeBSD, etc. This is achieved by reducing the amount of dependencies to just a C compiler, which exists pretty much for any platform these days.
- You end up using the same language for developing and building your project. Which may enable some interesting code reusage strategies. The build system can use the code of the project itself directly and the project can use the code of the build system also directly.
- You get to use C more.
- ...

## Disadvantages of NoBuild

- You need to be comfortable with C and implementing things yourself. As mentioned above this is like writing shell scripts but in C.
- It probably does not make any sense outside of C/C++ projects.
- You get to use C more.
- ...

## Why is it called "nobuild" when it's clearly a build tool?

You know all these BS movements that supposedly remove the root cause of your problems? Things like [NoSQL](https://en.wikipedia.org/wiki/NoSQL), [No-code](https://en.wikipedia.org/wiki/No-code_development_platform), [Serverless](https://en.wikipedia.org/wiki/Serverless_computing), etc. This is the same logic. I had too many problems with the process of building C projects. So there is nobuild anymore.

## How to use the library in your own project

The only file you need from here is [nob.h](https://raw.githubusercontent.com/tsoding/nob.h/refs/heads/main/nob.h). Just copy-paste it to your project and start using it. See [how_to/](how_to/) folder for examples.
