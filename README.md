# MTurnhout.github.io

## Development

### Requirements

 - [Docker](https://www.docker.com/)
 - [Visual Studio Code](https://code.visualstudio.com/)
   - [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

### Get Started

Opening the root folder in VSCode will automatically load the development container.  
First time use following command in the terminal to install required gems:

```
bundle install
```

Use the following command in the terminal to serve Jekyll website with live reload:

```bash
bundle exec jekyll serve --drafts --force_polling --livereload
```

Use the following command in the command palette to stop the development container:

```
Remote: Close Remote Connection
```
