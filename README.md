# @sxwy/semantic-release-template

## 用法

### 单包仓库

1. 配置 `package.json`

    ```json
    {
      "scripts": {
        "release": "semantic-release"
      },
      "devDependencies": {
        "@semantic-release/changelog": "^6.0.1",
        "@semantic-release/git": "^10.0.1",
        "semantic-release": "^19.0.3"
      }
    }
    ```

2. 配置 `releaserc.js`

    ```js
    module.exports = {
      plugins: [
        '@semantic-release/commit-analyzer',
        '@semantic-release/release-notes-generator',
        [
          '@semantic-release/changelog',
          {
            changelogFile: 'CHANGELOG.md'
          }
        ],
        '@semantic-release/github',
        '@semantic-release/npm',
        [
          '@semantic-release/git',
          {
            assets: ['CHANGELOG.md', 'package.json']
          }
        ]
      ]
    }
    ```

3. 配置 .npmrc

    ```
    registry=https://registry.npmjs.org/
    access=public
    ```

4. 配置 `.github/workflows/publish.yml`

    ```
    name: publish

    on:
      push:
        branches:
          - master
          - beta

    jobs:
      publish:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout
            uses: actions/checkout@v3

          - name: UseNode
            uses: actions/setup-node@v3
            with:
              node-version: 16

          - name: Publish
            env:
              GH_TOKEN: ${{ secrets.GH_TOKEN }}
              NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
            run: |
              yarn
              yarn release
    ```

5. 然后合并代码到 beta、master 等分支自动触发发布

### 多包仓库

1. 配置项目 `package.json`

    ```json
    {
      "name": "xxx",
      "workspaces": [
        "packages/*",
      ],
      "scripts": {
        "release": "multi-semantic-release --deps.bump=satisfy --deps.release=patch"
      },
      "devDependencies": {
        "@semantic-release/changelog": "^6.0.1",
        "@semantic-release/git": "^10.0.1",
        "multi-semantic-release": "^2.13.0",
        "semantic-release": "^19.0.3"
      }
    }
    ```

2. 配置 `releaserc.js`

    ```js
    module.exports = {
      plugins: [
        '@semantic-release/commit-analyzer',
        '@semantic-release/release-notes-generator',
        [
          '@semantic-release/changelog',
          {
            changelogFile: 'CHANGELOG.md'
          }
        ],
        '@semantic-release/github',
        '@semantic-release/npm',
        [
          '@semantic-release/git',
          {
            assets: ['CHANGELOG.md', 'package.json']
          }
        ]
      ]
    }
    ```

3. 配置 .npmrc

    ```
    registry=https://registry.npmjs.org/
    access=public
    ```

4. 配置 `.github/workflows/publish.yml`

    ```
    name: publish

    on:
      push:
        branches:
          - master
          - beta

    jobs:
      publish:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout
            uses: actions/checkout@v3

          - name: UseNode
            uses: actions/setup-node@v3
            with:
              node-version: 16

          - name: Publish
            env:
              GH_TOKEN: ${{ secrets.GH_TOKEN }}
              NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
            run: |
              yarn
              yarn release
    ```

5. 然后合并代码到 beta、master 等分支自动触发发布
