# Angular Google Tag Manager Service

A service library for integrate google tag manager in your angular project
This library was generated with [Angular CLI](https://github.com/angular/angular-cli)
For more info see this [how to install google tag manager article](https://itnext.io/how-to-add-google-tag-manager-to-an-angular-application-fc68624386e2)

## Getting Started

After installing it you need to provide your GTM id in app.module.ts

```
    providers: [
        ...
        {provide: 'googleTagManagerId',  useValue: YOUR_GTM_ID}
    ],
```

Or use the module's `forRoot` method

```
import { GoogleTagManagerModule } from 'angular-google-tag-manager';

imports: [
    ...
    GoogleTagManagerModule.forRoot({
      id: YOUR_GTM_ID,
    })
]
```

Or use the standalone based provider

```
import { provideGoogleTagManager } from 'angular-google-tag-manager';

export const appConfig: ApplicationConfig = {
  providers: [
    ...
    provideGoogleTagManager({
      id: 'YOUR_GTM_ID',
      gtm_csp_none: 'CSP-NONCE',
      // gtm_auth: YOUR_GTM_AUTH,
      // gtm_preview: YOUR_GTM_RESOURCE_PATH
      // gtm_resource_path:
      // gtm_mode: "silent" | "noisy"
    }),
  ],
};
```

Or use the `APP_INITIALIZER`

```
import { GoogleTagManagerConfiguration } from 'angular-google-tag-manager-config.service';

imports: [
    ...
    GoogleTagManagerModule.forRoot()
]

providers: [
    {
      ...
      provide: APP_INITIALIZER,
      useFactory: configInitializer,
      deps: [
        HttpBackend,
        GoogleTagManagerConfiguration,
      ],
      multi: true,
    },
  ],
```

set the config in the method assigned to useFactory

```
googleTagManagerConfiguration.set(googleTagManagerConfiguration);
```

inject the gtmService in your controller

```
constructor(
        ...
        private gtmService: GoogleTagManagerService,
    ) { }
```

then you can start pushing events on your gtm

```
 this.router.events.forEach(item => {
            if (item instanceof NavigationEnd) {
                const gtmTag = {
                    event: 'page',
                    pageName: item.url
                };

                this.gtmService.pushTag(gtmTag);
            }
        });
```

if you want to recive tags without pushing events simply call the function to enable it

```
    this.gtmService.addGtmToDom();
```

### Installing

In your Angular project run

```
npm i --save  angular-google-tag-manager
```

### Custom configuration and GTM environments

You can pass _gtm_preview_ and _gtm_auth_ optional variables to your GTM by providing them in app.module.ts

```
    providers: [
        ...
        {provide: 'googleTagManagerId',  useValue: YOUR_GTM_ID},
        {provide: 'googleTagManagerAuth',  useValue: YOUR_GTM_AUTH},
        {provide: 'googleTagManagerPreview',  useValue: YOUR_GTM_ENV},
        {provide: 'googleTagManagerResourcePath',  useValue: YOUR_GTM_RESOURCE_PATH},
        {provide: 'googleTagManagerCSPNonce',  useValue: YOUR_CSP_NONCE},
        {provide: 'googleTagManagerMode', useValue: "silent" | "noisy"}
    ],
```

Or using `forRoot`

```
import { GoogleTagManagerModule } from 'angular-google-tag-manager';

imports: [
    ...
    GoogleTagManagerModule.forRoot({
      id: YOUR_GTM_ID,
      gtm_auth: YOUR_GTM_AUTH,
      gtm_preview: YOUR_GTM_ENV
    })
]
```

## Authors

- **Marco Zuccaroli** - _Initial work_ - [Marco Zuccaroli](https://github.com/mzuccaroli)

See also the list of [contributors](https://github.com/mzuccaroli/angular-google-tag-manager/graphs/contributors) who participated in this project.

## License

This project is licensed under the MIT License

## Development

### GitHub Actions Workflows

This project includes automated CI/CD workflows:

#### CI Workflow
- Runs on every push to `main` branch and pull requests
- Executes linting, tests, and builds
- Ensures code quality before merging

#### Publish Workflow
- Triggers when a new version tag is pushed (e.g., `v1.12.0`)
- Automatically publishes the package to npm
- Creates a GitHub release
- Requires `NPM_TOKEN` secret to be configured

### Setting up NPM Token

To enable automatic publishing, add your npm token as a GitHub secret:

1. Generate an npm access token at [npmjs.com](https://www.npmjs.com/settings/tokens)
2. Go to your GitHub repository → Settings → Secrets and variables → Actions
3. Add a new secret named `NPM_TOKEN` with your npm token value

### Publishing a New Version

1. Update the version in `package.json` and `projects/angular-google-tag-manager/package.json`
2. Commit and push your changes
3. Create and push a new tag: `git tag v1.12.0 && git push origin v1.12.0`
4. The workflow will automatically build, test, and publish the package

## Acknowledgments

- Thanks to PurpleBooth for the [Readme Template](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2)
