## Develop the handbook locally
This project uses [`mdBook`](https://github.com/rust-lang/mdBook) to create the deployed version of the handbook. 
To install `mdBook` on your device please refer to this [installation guide](https://rust-lang.github.io/mdBook/guide/installation.html).


Once mdBook is installed on your computer, you can navigate to the project's root folder and run

``` bash
mdbook build --open
```
to build the handbook and open it in your deafult browser.

During development you can use 
``` bash
mdbook serve
```
command which will launch an HTTP server and serve the content at `localhost:3000`. It watches the book’s `src` directory for changes, rebuilding the book and refreshing clients for each change.

For more information you can refer to the `mdBook` [documentation](https://rust-lang.github.io/mdBook/index.html).

## Contribute to this repo
If you feel like you can add anything useful — whether it’s a new feature, a bug fix, or an improvement — feel free to contribute! [Just fork the repo, make your changes, and open a pull request](https://docs.github.com/en/get-started/exploring-projects-on-github/contributing-to-a-project). Every bit of help is appreciated! 🚀