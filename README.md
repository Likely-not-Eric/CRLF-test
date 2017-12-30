# CRLF-test

**Firstly, set aside your existing system and global .gitconfig (minimize the test matrix).**

In any case you'll need to have your `core.autocrlf` set *before* you check out your master branch simply cloning.

## Setup

### Simple way

Using the global configuration - this requires less setup and simpler workflows but will make side-by-side comparison and experimation more difficult.

    git config --global core.autocrlf input
    git clone https://github.com/Likely-not-Eric/CRLF-test.git CRLF-test
    xxd CRLF-test\text.auto

### More interesting way

    git init CRLF-test-autocrlf_input
    git -C CRLF-test-autocrlf_input config core.autocrlf input
    git -C CRLF-test-autocrlf_input remote add origin https://github.com/Likely-not-Eric/CRLF-test.git
    git -C CRLF-test-autocrlf_input fetch origin
    git -C CRLF-test-autocrlf_input checkout master
    xxd CRLF-test-autocrlf_input\text.auto

    git init CRLF-test-autocrlf_auto
    git -C CRLF-test-autocrlf_auto config core.autocrlf true
    git -C CRLF-test-autocrlf_auto remote add origin https://github.com/Likely-not-Eric/CRLF-test.git
    git -C CRLF-test-autocrlf_auto fetch origin
    git -C CRLF-test-autocrlf_auto checkout master
    xxd CRLF-test-autocrlf_auto\text.auto

    git init CRLF-test-autocrlf_false
    git -C CRLF-test-autocrlf_false config core.autocrlf false
    git -C CRLF-test-autocrlf_false remote add origin https://github.com/Likely-not-Eric/CRLF-test.git
    git -C CRLF-test-autocrlf_false fetch origin
    git -C CRLF-test-autocrlf_false checkout master
    xxd CRLF-test-autocrlf_false\text.auto
    
## Results

Expect "true" and "false" to both result the following contents of `text.auto`:

    00000000: 4c69 6e65 2031 0d0a 4c69 6e65 2032 0d0a  Line 1..Line 2..
    00000010: 4c69 6e65 2033 0d0a                      Line 3..

Expect "input" to result in the following contents of `text.auto`:

    00000000: 4c69 6e65 2031 0a4c 696e 6520 320a 4c69  Line 1.Line 2.Li
    00000010: 6e65 2033 0a                             ne 3.

Note the differences in line endings.
