---
layout: post
title:  "Overbloated hello world"
date:   2018-07-02 20:45:00 +0200
categories: jekyll update
---
I wrote an overbloated "Hello, world!" in C++.

I wanted to write a example application which would parse command line arguments
and execute a command.

I wanted to use some super simple solution. I know there are libs for that, but I wanted
to have a simple code snippet, which will rely only on STL and woulnd't introduce any
additional dependency.

I used for that std::vector and std::string.

std::vector has a constructor which can be initialized from raw array pointers:
```c++
using CmdArgs = std::vector<std::string>;

CmdArgs cmd_args(argv + 1, argv + argc);
```

std::string allows to quickly compare the strings stored in the arguments vector:
```c++
arg == "-h" || arg == "--help"
```

I didn't use exceptions during parsing of the command line arguments, because I would like to
have a solution for embedded system environment, which does not allow exceptions. For the help
message I used a raw string, this allows me to format the help message exactly as I would like
it to appear:

```c++
const char* const help_message = R"_(Welcome to Command Line Interface Tool!

Usage info:
    -h or --help    - print usage information
    -H or --hello   - print hello message
)_";
```

The options for the application I simply store in a struct:
```c++
struct Arguments
{
    bool print_hello = false;
};
```

Full code:

```c++
#include <iostream>
#include <string>
#include <vector>

using CmdArgs = std::vector<std::string>;

const char* const help_message = R"_(Welcome to Command Line Interface Tool!

Usage info:
    -h or --help    - print usage information
    -H or --hello   - print hello message
)_";

void print_help()
{
    std::cout << help_message << std::endl;
}

struct Arguments
{
    bool print_hello = false;
};

Arguments parse_args(const CmdArgs& args)
{
    Arguments app_args;
    for (const auto& arg : args)
    {
        if (arg == "-h" || arg == "--help")
        {
            print_help();
            std::exit(0);
        }
        else if (arg == "-H" || arg == "--hello")
        {
            app_args.print_hello = true;
        }
        else
        {
            std::cout << "Unknown argument: "
            		  << arg
            		  << ". Check available options below.\n\n";
            print_help();
            std::exit(1);
        }
    }

    return app_args;
}

int main(int argc, char *argv[])
{
    if (argc == 1)
    {
        std::cout << "Please provide an argument!\n" << std::endl;
        print_help();
        std::exit(1);
    }

    CmdArgs cmd_args(argv + 1, argv + argc);

    auto app_args = parse_args(cmd_args);

    if (app_args.print_hello)
        std::cout << "Hello, world!" << std::endl;

    return 0;
}
```