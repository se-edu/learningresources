<frontmatter>
  title: ESLint
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# ESLint

Author: [Nicholas Chua](https://github.com/nus-cs3281/2018/blob/master/students/nicholasChua/nicholasChua-Resume.md)

## Overview

[ESLint](https://eslint.org/docs/about/) is an open source JavaScript linter.

Code linting is a type of static analysis that helps find problematic patterns or code that does not follow a certain style guideline. JavaScript, being a loosely-typed, interpreted language, is prone to developer errors and therefore linting is especially important when working with JavaScript.

ESLint is shipped with a wide range of [built-in rules](https://eslint.org/docs/rules/) that can be configured on installation. It also offers flexibility by allowing importing your own custom rules via [plugins](https://eslint.org/docs/developer-guide/working-with-plugins).

## Rules
ESLint comes shipped with [built-in rules](https://eslint.org/docs/rules/) that can be [configured](#setting-up) to suit your needs.

An example of a rule would be [`brace-style`](https://eslint.org/docs/rules/brace-style). It enforces a specific placement of braces relative to the control statement and body. Brace style is based on preference or organization standards, and ESLint has options to enforce the brace style of your choice. They include:

[One true brace style (1TBS)](https://en.wikipedia.org/wiki/Indentation_style#Variant:_1TBS_(OTBS))
```js
if (foo) {
  bar();
} else {
  baz();
}
```

[Stroustrup](https://en.wikipedia.org/wiki/Indentation_style#Variant:_Stroustrup)
```js
if (foo) {
  bar();
}
else {
  baz();
}
```

[Allman](https://en.wikipedia.org/wiki/Indentation_style#Allman_style)
```js
if (foo)
{
  bar();
}
else
{
  baz();
}
```

An additional option `allowSingleLine` can be enabled to allow opening and closing braces for a block to be on the same line:
```js
if (foo) { bar(); }
else { baz(); }
```

If you are unsure about the options of the rule you want to change, you can refer to the comprehensive [rule documentation](https://eslint.org/docs/rules/).

## Setting Up

After [installing with npm](https://eslint.org/docs/user-guide/getting-started#installation-and-usage), ESLint can be configured in [two ways](https://eslint.org/docs/user-guide/configuring#configuring-rules):

**Single configuration file**. In this example, the rule is configured in `.eslintrc.json` and enforces all braces in the **project** to follow Allman style:
```json
{
  "rules": {
    "brace-style": ["error", "allman"]
  }
}
```

**Configuration comments** inside your file. In this example, the comments enforces all braces in the **file** to follow Allman style:
```js
/* eslint brace-style: ["error", "allman"] */
if (foo === bar) { // should be on the next line
  return;
}
```
Linting output:
```shell
 1:10  error  Opening curly brace appears on the same line as controlling statement  brace-style
```
<br/>

**NOTE**: That specifying the rule option as "error" will result in the exit code of 1 when the rule is violated. If you wish for ESLint to alert you but not fail the linting, you may set it to "warn":

```js
/* eslint brace-style: ["warn", "allman"] */
if (foo === bar) { // warning displayed but does not affect exit code
  return;
}
```
Linting output:
```shell
1:10  warning  Opening curly brace appears on the same line as controlling statement  brace-style
```
<br/>

If you are not sure what rules to add to your project, you can import [configurations](https://eslint.org/docs/developer-guide/shareable-configs) published by more experienced developers or organizations.

Here are some popular style guide's configurations:
* [Airbnb](https://www.npmjs.com/package/eslint-config-airbnb)
* [Standard](https://www.npmjs.com/package/eslint-config-standard)
* [Google](https://www.npmjs.com/package/eslint-config-google)

At the same time, ESLint allows you to disable rules for [specific files and directories](https://eslint.org/docs/user-guide/configuring#ignoring-files-and-directories) or for [specific lines](https://eslint.org/docs/user-guide/configuring#disabling-rules-with-inline-comments).

## Usage
Once configured, you can lint your files with [terminal commands](https://eslint.org/docs/user-guide/command-line-interface) and you can add [options](https://eslint.org/docs/user-guide/command-line-interface#options) to the command based on your needs. A useful example would be the `--fix` flag which can be used to fix all fixable errors in your code.
```
$ eslint --fix file.js
```
<br/>

ESLint can be easily integrated with your build tool of choice. Here are some [plugins](https://eslint.org/docs/user-guide/integrations#build-tools) for some popular build tools. ESLint can also work with Continuous Integration tools like Travis, AppVeyor and CircleCI. Configuration is explained in this [example using Travis](https://medium.com/jason-feng/travis-ci-guide-bdc03b3dbc9d). You can also learn how to automate ESLint to [run every time you commit](https://medium.com/the-node-js-collection/why-and-how-to-use-eslint-in-your-project-742d0bc61ed7).

<br/>

ESLint also has [editor integrations](https://eslint.org/docs/user-guide/integrations#editors) such as error highlighting in many popular editors like Atom and WebStorm.

[ESLint Package for Atom](https://atom.io/packages/linter-eslint)

![linter-eslint](https://imgur.com/6Nrj4NV.png)

## Plugins
Custom rules can be [imported](https://eslint.org/docs/user-guide/configuring#using-the-configuration-from-a-plugin) via Plugins. You can [learn how to write your own](https://eslint.org/docs/developer-guide/working-with-plugins) or [use existing ones](https://www.npmjs.com/browse/keyword/eslintplugin) that are already published to npm.

A popular plugin would be [eslint-plugin-lodash](https://github.com/wix/eslint-plugin-lodash) which is meant to enforce good practices and styling with the utility library [Lodash](https://lodash.com/). You can checkout other plugins [here](https://github.com/dustinspecker/awesome-eslint#plugins).

## Resources
* [ESLint](https://eslint.org/)
* [ESLint Github](https://github.com/eslint/eslint): You can contribute to the project but remember to read their [guidelines](https://eslint.org/docs/developer-guide/contributing/) first
* [npm Developer Guide](https://docs.npmjs.com/misc/developers): If you're writing your own custom rules/configs, you need to know how to publish them on npm.
* [Awesome ESLint](https://github.com/dustinspecker/awesome-eslint): repository for ESLint configs, plugins and other useful links.

</div>
