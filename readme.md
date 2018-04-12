# CNTN

This is a ChiNese Text Normalization (CNTN) tool for Text-to-speech system, which is based on [sparrowhawk](https://github.com/google/sparrowhawk). The main purpose of this project is to provide some `grammar` files for Chinese TN, rather than modify the source code of sparrowhawk or show how to integrade it to a TTS system.

WIP Note: Work In Progress

## Install

You could follow the steps in file [install_sparrowhawk.md](help/install_sparrowhawk.md) to install sparrowhawk. In addition, for a quick startup, we also provide pre-built binaries under the [sparrowhawk directory](./sparrowhawk) and required libraries. However, if these binaries could run properly in you PC, that may be caused by your system environment, you should built it manually.

## Compile grammar

First, prepare enviroment:
```
cd sparrowhawk/bin
. bin/path.sh
```

English:
```
cd grammars/en/en_toy/classify

thraxmakedep tokenize_and_classify.grm
make

cd ../verbalize/
thraxmakedep verbalize.grm
make
```

## Run

```shell
cd grammars/en/
normalizer_main \
--config=sparrowhawk_configuration.ascii_proto \
--multi_line_text \
< test.txt \
2>/dev/null
```

Note: The `--path_prefix` can not be applied to the `sentence_boundary_exceptions.txt` file. This may be a bug. That's why we should cd into the grammar dir rathar use the `--path_prefix` flag.

If you want to debug the grammars, the `thraxrewrite-tester` command is recommended, for example:

```
thraxrewrite-tester --far=en_toy/classify/word.far --rules=WORD
```