# 🧑‍🚀 Pilgrim Theme for Vitepress

Pilgrim is a documentation theme for Vitepress.

To use this theme in your own Vitepress project

In your `.vitepress/theme/index.ts`:

```ts
import { Theme } from '@hempworks/pilgrim'

export default Theme
```

Customize the configuration for your uses inside `.vitepress/config.ts`:

```ts
import { defineConfigWithTheme } from 'vitepress'
import type { ThemeConfig } from '@hempworks/pilgrim'
import config from '@hempworks/pilgrim/config'

export default defineConfigWithTheme<ThemeConfig>({
  extends: config,
  title: 'Laravel Nova',
  description: 'Master Your Universe',
  base: '/docs',
  srcDir: 'src',

  themeConfig: {
    logo: {
      light: '/logo.svg',
      dark: '/logo-dark.svg',
    },
    nav: [
      { text: 'Docs', link: 'https://nova.laravel.com/docs' },
      { text: 'Releases', link: 'https://nova.laravel.com' },
      { text: 'Course', link: 'https://laracasts.com/' },
    ],
    githubUrl: 'https://github.com/laravel/nova-issues',
    showVersionPicker: true,
    versions: [
      { text: 'v1.0', link: '/docs/1.0' },
      { text: 'v2.0', link: '/docs/2.0', current: true },
    ],
    sidebar: [
      {
        text: 'Getting Started',
        items: [
          { text: 'Installation', link: '/installation' },
          { text: 'Release Notes', link: '/releases' },
          { text: 'Upgrade', link: '/upgrade' },
          { text: 'Support', link: '/support' },
          { text: 'Code of Conduct', link: '/code-of-conduct' },
        ],
      },
    ],
    search: {
      provider: 'algolia',
      options: {
        indexName: '',
        appId: '',
        apiKey: '',
        placeholder: 'Search Nova Docs...',
      },
    },
  },
})
```
