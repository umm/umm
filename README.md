# What is umm?

**umm** is an abbreviation **U**nity **M**odule **M**anager, a management system for assets of Unity.

We call various assets aggregation as `modules`, and manage thier dependencies using [yarn](https://yarnpkg.com/) which is an upward compatible tool of [npm](https://docs.npmjs.com/) which is a package management system in [Node.js](https://nodejs.org/).

## How to use?

### To construct environment to use `umm`

Will explain the environment construction method to use umm.

1. Install [Node.js](https://nodejs.org/).
    * It is a good idea to install the latest version.
1. Install [yarn](https://yarnpkg.com/).

### To retrieve modules

Will explain how to retrieve some modules in a Unity project already module managed by `umm`.

1. Run `yarn install` command in the root directory of the Unity project for which you want to retrieve the modules.
1. Modules are deploying under the `Assets/Modules/` of the Unity project automatically.

### To add modules

Will exprain how to add a module into a Unity project.

1. Run `yarn install` command in the root directory of the Unity project for which you want to add the module.
1. Module is deploying under the `Assets/Modules/` of the Unity project, also create of update the files which named `package.json` and `yarn.lock`. 
    * I strongly recommend to version control each manifest files which manage information of modules.

#### Name of modules

Module name specification method differs according to module publication destination.

##### Specify the npm registry such as npmjs.org

```shell
yarn add @$SCOPE/$MODULE
```

##### Specify the git repository such as GitHub

```shell
yarn add github:$ORGANIZATION/$REPOSITORY
```

##### Specify the tar ball URL directly

```shell
yarn add $TAR_BALL_URL
```

##### Specify the path to some local storage

```shell
yarn add file:$PATH_TO_MODULE
```

#### Versioning

Specify the version to be installed according to [SemVer](https://semver.org)'s rules

Please check [the document of npm](https://docs.npmjs.com/files/package.json#dependencies) to specify the range of versions.

You can specify a range of versions for a git repository like GitHub if you use yarn.

```shell
yarn add "umm/cafu_core#^1.0.0"
                        ^^^^^^
```

### To create modules

#### Create `.umm-config.json`

To create umm modules needs configuration file names `.umm-config.json`.

Replace placeholders in template when initialize module accoring to this file.

Write the configuration like below and put to your home directory.

```json
{
  "scopes": {
    "@monry": {
      "repository": {
        "type": "<REPOSITORY_TYPE>",
        "host": "<REPOSITORY_HOST>",
        "group": "<REPOSITORY_GROUP>",
        "user": "<REPOSITORY_USER>"
      },
      "author": {
        "name": "<AUTHOR_NAME>",
        "email": "<AUTHOR_EMAIL>",
        "url": "<AUTHOR_HOMEPAGE>"
      },
      "license": "<DEFAULT_LICENSE>"
    }
  }
}
```

Means of parameters are defined below table.

| Parameter | Example | Description |
| --- | --- | --- |
| REPOSITORY_TYPE | `git` | Specify type of repository. Currently supported only `git` 😓 |
| REPOSITORY_HOST | `github.com` | Specify hostname of repository. |
| REPOSITORY_GROUP | `monry` | Specify name of user or organization of repository. |
| REPOSITORY_USER | `git` | Specify ssh username of repository. |
| AUTHOR_NAME | `monry` | Specify name of author. |
| AUTHOR_EMAIL | `monry@example.com` | Specify email of author. |
| AUTHOR_HOMEPAGE | `https://github.com/monry` | Specify homepage of author. |
| DEFAULT_LICENSE | `MIT` | Specify default license. |

#### Prepare modules

1. Executing the following command creates a template for the module.
    * Thanks @mattak !!

```shell
bash <(curl -L https://gist.githubusercontent.com/mattak/f95a8f4c8750ee61aea79ec09d87f659/raw/cb49f66f5a7747fa700b2e8fb57f386969e64b49/umm-create.sh)
```

2. You can develop the module like normally Unity project.
    * Of cource, you can use other umm modules.
2. Publish the module into some repository like GitHub.
    * But, you must deploy into private repository if your module includes paid assets or private codes.
    * You can contribute as contributer of umm if you want, so you can publish module in [umm organizations](https://github.com/umm).
    * I don't recommend to publish to npmjs.org with use `npm publish` command, because assets is not codes of Node.js.

#### Distribution destination of modules

The module can publishes into any [destination which supported by npm](https://docs.npmjs.com/files/package.json#dependencies), such as [GitHub](https://github.com/), [BitBucket](https://bitbucket.com) and local storage.

#### Versioning of modules

Versioning according to [SemVer](https://semver.org)'s rules.

If you publish a module to GitHub, you can think of it as a module version by setting the tag of SemVer notation as `git tag`.

By using the `npm version` command, you can changes version of `package.json` and run `git tag` command at the same time.

## Links

[README.md (Japanese)](README.ja.md)
