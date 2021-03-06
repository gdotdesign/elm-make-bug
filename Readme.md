Repository for reproducing the https://github.com/elm-lang/elm-make/issues/107 bug.

Script to colne the repository and test the bug: 
- it should return **1** for correct behavior and **0** for erroneous behavior
- bash script (by [note89](https://github.com/note89)) https://gist.github.com/gdotdesign/b0c21b2cae66518f34d417cde7613576
- windows script (by [ManuScriptHub](https://github.com/ManuScriptHub)) https://gist.github.com/gdotdesign/e53fac955f98201212c8f18837e12bcc

Steps to reproduce:
* `elm-make Test.elm` - This will compile fine
  
  ```
  Success! Compiled 4 modules.
  Successfully generated index.html
  ```
* Change `Lib.elm` from `type Msg = A` to `type Msg = B`
* `elm-make Test.elm` - This will throw the following 2 errors:
  ```
  -- NAMING ERROR ---------------------------------------------------- ././Bar.elm

  Cannot find variable `Lib.A`.

  5| x = Lib.A
         ^^^^^
  `Lib` does not expose `A`. Maybe you want one of the following?

      Lib.B

  ==================================== ERRORS ====================================

  -- NAMING ERROR ---------------------------------------------------- ././Foo.elm

  Cannot find variable `Lib.A`.

  5| x = Lib.A
         ^^^^^
  `Lib` does not expose `A`. Maybe you want one of the following?

      Lib.B

  Detected errors in 2 modules.
  ```
* Fix the problem in `Bar.elm` by changing `x = Lib.A` to `x = Lib.B`
* `elm-make Test.elm` - This will compile fine but the error in `Foo.elm` is not
  fixed and because of this it will throw a **runtime error**.
  
  ```
  Success! Compiled 2 modules.
  Successfully generated index.html
  ```
* From this point running `elm-make Test.elm` will run file without compiling
  anything.
