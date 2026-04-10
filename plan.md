## content type
### page/article
- title
- images
- section
    related to section
- content
    rich text
- type
    - home
    - about
    - contact
- code
    - long text
### section
- name
- description
- page
    relationship
-path

### thangka type/Category
- name
-decription
- thangkas
    relationship
- image
- order

### thangka
- name
- decription
- is featured
- image
-thangka type
    relationship

## settings
### add s3 support
```
    "@strapi/plugin-cloud": "5.38.0",
    "@strapi/plugin-users-permissions": "5.38.0",
    "@strapi/provider-upload-aws-s3": "^5.41.1",
```
### .env
AWS_ACCESS_KEY_ID=AKIA6JMLYZXPUINTJ2HS
AWS_SECRET_ACCESS_KEY=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
AWS_REGION=us-east-2
AWS_BUCKET=thangka-982230027743-us-east-2-an

### config/plugins.ts
```js
import type { Core } from '@strapi/strapi';

const config = ({ env }: Core.Config.Shared.ConfigParams): Core.Config.Plugin => ({
  upload: {
    config: {
      provider: 'aws-s3',
      providerOptions: {
        accessKeyId: env('AWS_ACCESS_KEY_ID'),
        secretAccessKey: env('AWS_SECRET_ACCESS_KEY'),
        region: env('AWS_REGION'),
        params: {
          Bucket: env('AWS_BUCKET'),
          ACL: null,
        },
      },
      actionOptions: {
        upload: {
          ACL: null,
        },
        uploadStream: {
          ACL: null,
        },
        delete: {},
      },
    },
  },
});

export default config;
```
### config/middlewares.ts
```js
import type { Core } from '@strapi/strapi';

const config: Core.Config.Middlewares = [
  'strapi::logger',
  'strapi::errors',
  'strapi::security',
  {
    name: 'strapi::security',
    config: {
      contentSecurityPolicy: {
        directives: {
          'img-src': [
          "'self'",
          'data:',
          'blob:',
          'https:',
          ],
        },
      },
    },
  },

  'strapi::cors',
  'strapi::poweredBy',
  'strapi::query',
  'strapi::body',
  'strapi::session',
  'strapi::favicon',
  'strapi::public',
];

export default config;
```
