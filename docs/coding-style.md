# Coding style

<!-- TODO: add a link to the coding style to the PR template -->

We loosely follow the [GNU Coding Standards](https://www.gnu.org/prep/standards/standards.html) with some local deviations. Whether you agree with them or not, do check it out—it is an educational read. In a nutshell:

* Use templates for new files (see [maint/templates]({{ config.repo_url }}/tree/master/maint/templates) in the source tree)
* Maximum line width is 100 characters[^1]
* No tabs, indent with 4 spaces
* No trailing whitespace

Use the `clang-format` to format the code:

```shell
$ make indent
```

To avoid formatting differences between `clang-format` versions, we currently use `clang-format-19`. The [clang-format Python distribution](https://pypi.org/project/clang-format/) provides precompiled binaries for all major platforms:

```shell
$ uv tool install 'clang-format==19.*'
$ alias clang-format='uvx clang-format'
```

[^1]: This is not to please folks with low-resolution screens, but rather because sticking to 100 columns prevents you from easily nesting more than one level of if statements or other code blocks.

## Readable code

Use your best judgment and choose the more readable option. Remember that many other people will be reading it:

```c title="Right"
bytes = read (fd, &routine.pointer, sizeof (routine));
if (bytes == -1 || (size_t) bytes < sizeof (routine))
    ...
```

```c title="Wrong"
if ((bytes = read (fd, &routine.pointer, sizeof (routine))) == -1 || (size_t) bytes < sizeof (routine))
    ...
```

Do not put more than one statement on a line:

<div class="grid" markdown>
```c title="Right"
a = 0;
b = 2;

a = f ();
if (a == 2)
    b = 5;
```

```c title="Wrong"
a = 0; b = 2;

if ((a = f()) == 2)
    b = 5;

if (a == 2) b = 5;
```
</div>

Use explicit comparison in equality operators:

```c
void *p1, *p2;
int i1, i2;
char c1, c2;
```

<div class="grid" markdown>
```c title="Right"
if (p1 != NULL)
if (p2 == NULL)

if (i1 != 0)
if (i2 == 0)

if (c1 != '\0')
if (c2 == '\0')
```

```c title="Wrong"
if (p1)
if (!p2)

if (i1)
if (!i2)

if (c1)
if (!c2)
```
</div>

Do not check boolean values for equality:

```c
gboolean b1, b2;
```

<div class="grid" markdown>
```c title="Right"
if (b1)
if (!b2)
```

```c title="Wrong"
if (b1 == TRUE)
if (b2 == FALSE)
```
</div>

## Comments

Precede comments with a blank line. If the comment belongs directly to the following code, there should not be a blank line after the comment, unless the comment contains a summary of several blocks of following code.

```c title="Right"
/*
 * This is a multiline comment
 *
 * Note that edit_delete() will not corrupt anything if it is called 
 * while the cursor position is EOF.
 */
(void) edit_delete (edit);

// This is a one-line comment. Allocate additional memory.
mem = (char *) malloc (memneed);

/**
 * @brief This is a Doxygen comment
 *
 * This is a more detailed explanation of
 * this simple function.
 *
 * @param[in]   param1     The parameter value of the function
 * @param[out]  result1    The result value of the function
 * @return                 0 on success and -1 on error
 */
int example (int param1, int *result1);
```

```c title="Wrong"
//This is a one-line comment.
mem = (char *) malloc (memneed);// No space before comment

/* This is a multiline comment,
   with some more words...*/
```

## Conditionals

Always follow an `if` keyword with a space, but do not include additional spaces before or after the parentheses in the conditional:

<div class="grid" markdown>
```c title="Right"
if (i == 0)
⠀
```

```c title="Wrong"
if ( i == 0 )
if (0 == i)
```
</div>

## Function calls

Always include a space between the name and the left parentheses when calling functions:

<div class="grid" markdown>
```c title="Right"
do_example (int param1, int *result1);
```

```c title="Wrong"
do_example(int param1, int *result1);
```
</div>

## Braces

Braces for blocks of code associated with `for`, `if`, `switch`, `while`, `do .. while`, etc. should start on the next line after the statement keyword and end on a separate line.

If the length of the opening statement requires it to span multiple lines, the opening brace should be on a separate line.

Do not use braces unnecessarily when a single statement will do.

```c title="Right"
if (xterm_flag && xterm_title)
{
    path = strip_home_and_password (current_panel->cwd);
    ...
}

for (k = 0; k < 10; k++)
     for (j = 0; j < 10; j++)
        for (i = 0; str_options[i].opt_name != NULL; i++)
            g_free (*str_options[i].opt_addr);
```

```c title="Wrong"
if (xterm_flag && xterm_title) {
    path = strip_home_and_password (current_panel->cwd);
    ...
}

if (xterm_flag && xterm_title)
{
    path = strip_home_and_password (current_panel->cwd); }
```

## Goto

Use `goto` only when necessary; it is evil, but can greatly improve readability and reduce memory leaks when used as the only exit point from a function.

```c title="Right"
{
    if (link_type == LINK_HARDLINK)
    {
        src = g_strdup_printf (_ ("Link %s to:"), str_trunc (fname, 46));
        dest = input_expand_dialog (_ ("Link"), src, MC_HISTORY_FM_LINK, "");

        if (dest == NULL || *dest == '\0')
            goto cleanup;
        ...
        ...
    }
    ...
    ...

cleanup:
    g_free (src);
    g_free (dest);

}
```

## Variables

Do not mix variable declarations and code; declare variables only at the beginning of the appropriate block.

Reduce variable scope as much as possible: declare local variables in the block where they are used.

Separate variable declaration and code with an empty line.

<div class="grid" markdown>
```c title="Right"
{
    while (TRUE) {
        int foo = 0;

        do_bar (foo);
    }
}
```

```c title="Wrong"
{
    int foo = 0;
    while (TRUE) {
        do_bar (foo);
    }
}
⠀
```
</div>

If a variable is introduced only to store an intermediate value, declare it at the place of use, join declaration and initialization, and mark it as a constant:

<div class="grid" markdown>
```c title="Right"
const ssize_t len = mc_readlink ( ... );
⠀
⠀
```

```c title="Wrong"
ssize_t len;

len = mc_readlink ( ... );
```
</div>

Avoid having initialized and uninitialized variables in the same declaration:

<div class="grid" markdown>
```c title="Right"
int a;
int b = 0;
```

```c title="Wrong"
int a, b = 0;
⠀
```
</div>

Avoid multiple non-trivial variable initializations in a declaration:

<div class="grid" markdown>
```c title="Right"
int a = 2 + 5;
int b = 4 * 3 - 1;
```

```c title="Wrong"
int a = 2 + 5, b = 4 * 3 - 1;
⠀
```
</div>

Mark unused variables with the `MC_UNUSED` macro:

```c title="Right"
int
progress_button_callback (MC_UNUSED WButton *button, MC_UNUSED int action)
{
    return 0;
}
```

```c title="Wrong"
int
progress_button_callback (WButton *button, int action)
{
    (void) button;
    (void) action;

    return 0;
}
```

Try to avoid passing function calls as function parameters in new code. Not doing so makes the code much easier to read, and it is also easier to use the `step` command in `gdb`.

```c title="Right"
void
dirsizes_cmd (void)
{
    const ComputeDirSizeUI *ui = compute_dir_size_create_ui ();
    compute_dir_size_destroy_ui (ui);
}
```

```c title="Wrong"
void
dirsizes_cmd (void)
{
    compute_dir_size_destroy_ui (compute_dir_size_create_ui ());
}
```

Avoid abusing non-`const` function parameters as local variables:

```c title="Right"
void
foo (const int iterations)
{
    int result;

    result = do_one_thing (iterations);
    do_something (&result);
    ...
}
```

```c title="Wrong"
void
foo (int iterations)
{
    iterations = do_one_thing (iterations);
    do_something (&iterations);
    ...
}
```

## Loops

Declare loop variables within the loop to limit its scope and avoid unwanted reuse of the last value set.

<div class="grid" markdown>
```c title="Right"
⠀
for (int i = 0; i < 5; i++)
{
    do_something (i);
}
⠀
```

```c title="Wrong"
int i;

for (i = 0; i < 5; i++)
{
    do_something (i);
}
```
</div>

## Headers

Do not mix headers:

```c title="Right"
#include <errno.h>
#include <stdio.h>
#include <string.h>

#include <sys/types.h>
#include <sys/stat.h>

#include "lib/global.h"
#include "lib/tty/tty.h"  // LINES, tty_touch_screen()
#include "lib/tty/win.h"  // do_enter_ca_mode()

#include "src/subshell.h"  // use_subshell
#include "src/help.h"      // interactive_display()
#include "src/setup.h"
```

```c title="Wrong"
#include <errno.h>
#include <sys/types.h>
#include <stdio.h>
#include <string.h>

#include <sys/stat.h>

#include "src/subshell.h"  // use_subshell
#include "src/help.h"      // interactive_display()

#include "lib/tty/tty.h"  // LINES, tty_touch_screen()
#include "lib/tty/win.h"  // do_enter_ca_mode()

#include "src/setup.h"
#include "lib/global.h"
```

Use short comment for header file:

```c title="Right"
#include "lib/tty/tty.h"   // LINES, tty_touch_screen()
#include "lib/tty/win.h"   // do_enter_ca_mode()
#include "src/subshell.h"  // use_subshell
#include "src/help.h"      // interactive_display()
```
