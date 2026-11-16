# Welcome!

Welcome to the Firebot community plugin repository! This repo is designed to allow Firebot plugin developers to list their plugins within Firebot so that end users may quickly search for, install, and update them all within Firebot, providing plugin developers with better discoverability and more robust user support.

# Code of Conduct

This repository, along with all contributors and contributions to it, is governed by the [Firebot Community Code of Conduct](https://github.com/crowbartools/Firebot/blob/master/.github/CODE_OF_CONDUCT.md). Any violations of the Code of Conduct are subject to the enforcement actions listed therein.

# Content Moderation Policy

All plugin manifests submitted to this repository are subject to moderation by the owners of this repo. We may, at our sole discrection, refuse any plugin manifest contributions, or remove any published manifests, for any plugin or developer for any reason including, but not limited to, the following:

- Violations of the Code of Conduct as described above
- Prohibited Content as described below
- Distribution of malware, intentional or incidental
- Concerns about end user safety or privacy

# Prohibited Content

Plugin developers are ultimately repsonsible for the content in their plugins and manifests. Any plugin manifests submitted to this repository that contain prohibited content, whether in the manifest itself or the plugin it references, will not be accepted, and the developers thereof will be subject to enforcement actions as defined in the Code of Conduct. Prohibited content is as follows:

- Any content that violates the Twitch [Community Guidelines](https://safety.twitch.tv/s/article/Community-Guidelines), [Terms of Service](https://legal.twitch.com/legal/terms-of-service/), or [Developer Agreement](https://legal.twitch.com/legal/developer-agreement/)
- Plugins created using Generative AI, or which provide/facilitate Generative AI content. This includes, but is not limited to, the following types of content: text, image, audio, video, and code.
- Collection of end user data without consent

# Community Plugin Manifests

Community plugin manifests must be named `manifest.json` and stored in the directory structure format:

`manifests/{author-name}/{plugin-name}/{version}/manifest.json`

The `author-name` and `plugin-name` fields must only contain lowercase alphanumeric characters (`a-z` or `0-9`) or hyphens (`-`).

The `version` field must be a basic semver-compatible string that contains exactly 3 components: Major, minor, and patch. Version components must be separated by a period (`.`). Pre-release and/or metadata versions or version strings are not permitted, therefore there should be no preceeding or proceeding characters of any kind. This `version` field, the `version` field in your community manifest, and the `version` field in your plugin's internal manifest must all be identical.

Examples of **valid** manifest filenames:

- `manifests/zunderscore/throne/2.0.0/manifest.json`
- `manifests/oceanity/archipelago/1.0.10/manifest.json`

Examples of **invalid** manifest filenames:

- `manifests/PluginAuthor/plugin_name/1.0/manifest.json`
- `manifests/developer42/my-plugin/1.0.0.json`
- `manifests/cooldev/plugin-name/1.0.0-beta/manifest.json`

Manifests must conform to the following format:

```ts
interface CommunityPluginManifest {
    /**
     * Display name of your plugin (e.g. `My Cool Plugin`)
     */
    name: string;

    /**
     * Your name, as the author of this plugin (e.g. `Firebot`)
     */
    author: string;

    /**
     * A brief description of your plugin. Markdown is supported.
     */
    description: string;

    /**
     * Version of your plugin this manifest represents.
     * Must be a semver string with exactly 3 period-separated components:
     * Major, minor, and patch. (e.g. `1.0.0`)
     */
    version: string;

    /**
     * Direct download URL for the plugin file
     */
    downloadUrl: string;

    /**
     * SHA256 hash of the plugin file specified in `downloadUrl`.
     * Firebot will validate this hash after downloading your plugin.
     */
    sha256: string;

    /**
     * Release date of this version of your plugin.
     * Must be an ISO-8601 formatted string.
     */
    releaseDate: string;

    /**
     * Type of file your plugin is. Only `single-file` is supported at this time.
     */
    type: "single-file";

    /**
     * Optional. An icon representing your plugin. This can either be a Font Awesome icon
     * or a custom URL.
     */
    icon?: {
        /**
         * Indicates a Font Awesome icon
         */
        type: "font-awesome";

        /**
         * A FontAwesome icon name shown in the UI (eg. `fa-cogs`).
         */
        name: `fa-${string}`;

        /**
         * A CSS color value (e.g. `#FF0000`) used for the icon.
         */
        color?: string;
    } | {
        /**
         * Indicates a custom icon
         */
        type: "custom";

        /**
         * A URL for the icon image. This can be a data URL.
         */
        url: string;

        /**
         * A CSS color value (e.g. `#FF0000`) used for the background of the icon.
         */
        backgroundColor?: string;
    };

    /**
     * Optional. The category your plugin best fits under.
     */
    category?:
        | "stream-services"     // Stream Services
        | "social"              // Social
        | "music-media"         // Music & Media
        | "games"               // Games/Gaming
        | "overlays"            // Overlays
        | "tools-utilities";    // Tools & Utilities

    /**
     * Optional. An array of Firebot features that your plugin implements.
     * Each implemented feature only needs to be included in the array once.
     */
    features?: Array<
        | "effects"             // Effects
        | "events"              // Events
        | "variables"           // Variables
        | "integrations"        // Integrations
        | "overlay-widgets"     // Overlay Widgets
        | "games"               // Games
        | "commands"            // Commands
        | "ui-extensions"       // UI Extensions
    >;

    /**
     * Optional. An array of strings that describe or categorize your plugin.
     */
    tags?: string[];

    /**
     * Optional. A link to the plugin's source code repository.
     */
    repo?: string;

    /**
     * Optional. A link to the plugin's website. If the repo is on GitHub
     * and the website is not specified, it will default to the GitHub repo URL.
     */
    website?: string;

    /**
     * Optional. A link to the plugin's issue tracker or support server (e.g. Discord).
     * If the repo is on GitHub and support is not specified, it will default to
     * the GitHub issues URL.
     */
    support?: string;

    /**
     * Optional. The lowest Firebot version this version of your plugin is compatible with.
     */
    minimumFirebotVersion?: {
        major: number;
        
        /** Optional */
        minor?: number;
        
        /** Optional */
        patch?: number;
    };
    
    /**
     * Optional. The highest Firebot version this version of your plugin is compatible with.
     */
    maximumFirebotVersion?: {
        major: number;
        
        /** Optional */
        minor?: number;
        
        /** Optional */
        patch?: number;
    };
}
```

# Author Manifests

Optionally, you may also include an `author.json` file in your author directory (`manifests/{author-name}/author.json`) that provides more information about you as a plugin author. The `author.json` format is as follows:

> [!NOTE]
> The Twitch `username` and `userId` fields must correlate to the same account.

```ts
interface AuthorManifest {
    /**
     * Your name. This will usually be the same as your Twitch username and author directory name.
     * However, it may contain different capitalization (e.g. `DennisOnTheInternet`).
     */
    name: string;

    /**
     * Information about your Twitch account
     */
    twitch: {
        /**
         * Your Twitch user login name (e.g. `zunderscore`)
         */
        username: string;

        /**
         * Your Twitch account's internal user ID (e.g. `50989853`).
         * You can find this in your Firebot `auth-twitch.json` file.
         */
        userId: string;
    }
}
```

# Pull Requests

Pull requests must be created targeting the `main` branch.

PR titles must start with the `add` keyword followed by a colon and space, then include the author name, plugin name, and version as defined in the manifest file path, separated by the `/` character, as follows:

`add: {author-name}/{plugin-name}/{version}`

Example:

`add: zunderscore/fourthwall/2.0.0`

Or in the case of an author manifest:

`add: {author-name}/author.json`

Example:

`add: zunderscore/author.json`

Each PR may only contain a single manifest file. The only file that may be included in a PR is a **new** plugin manifest OR a **new** author manifest. Plugin manifests are **immutable**, so please ensure that any plugin manifest you submit via PR is complete and correct **BEFORE** opening your pull request. You **will not** be able to change a plugin manifest once it has been merged.

> [!NOTE]
> Author manifests _may_ be updated in the highly rare instance that your name or Twitch username changes, but your Twitch user ID may **not** be modified and any new Twitch username submitted must still correlate to the existing user ID both at the time of submission and review. Your author directory name inside  `manifests` may also **not** be changed as plugin manifests are immutable. This would also break plugin updates for end users who have already installed any of your plugins.