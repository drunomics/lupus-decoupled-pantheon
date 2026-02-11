# Lupus Decoupled Pantheon

A [Pantheon Custom Upstream](https://docs.pantheon.io/guides/custom-upstream) for deploying [Lupus Decoupled Drupal](https://lupus-decoupled.org) on Pantheon.

This upstream provides a ready-to-use Drupal backend powered by [lupus_decoupled_recipe](https://www.drupal.org/project/lupus_decoupled_recipe), configured for Pantheon's Integrated Composer workflow. The frontend (Nuxt.js) is deployed separately.

## Creating a Pantheon Site

### From the Pantheon Dashboard

Once this upstream is registered as a Custom Upstream in your Pantheon organization:

1. Go to your Pantheon dashboard
2. Click **Create New Site**
3. Select **Lupus Decoupled** from the upstream list
4. Follow the prompts to create your site

### From the CLI

```bash
terminus site:create my-site "My Site" lupus-decoupled-pantheon
```

### Installing Drupal

After creating the site, install Drupal using the Lupus Decoupled recipe:

```bash
terminus drush my-site.dev -- si --site-name='My Site' ../recipes/lupus_decoupled_recipe/standard
terminus drush my-site.dev -- en services_env_parameter -y
terminus drush my-site.dev -- config:set lupus_decoupled_ce_api.settings frontend_base_url 'https://your-frontend-url.com' -y
```

## Local Development with DDEV

This project includes [DDEV](https://ddev.readthedocs.io/) configuration for local development.

### Prerequisites

- [DDEV](https://ddev.readthedocs.io/en/stable/users/install/ddev-installation/) installed
- Docker or Colima running

### Setup

```bash
git clone git@github.com:drunomics/lupus-decoupled-pantheon.git
cd lupus-decoupled-pantheon
ddev start
ddev site-install
```

This will:
1. Start the DDEV environment
2. Install Composer dependencies
3. Clone and start the Nuxt.js frontend
4. Install Drupal with the Lupus Decoupled recipe

### Access

- **Drupal backend**: https://lupus-decoupled-pantheon.ddev.site
- **Nuxt frontend**: https://lupus-nuxt.ddev.site
- **Admin login**: `admin` / `lupus123`

## Frontend Deployment

Pantheon hosts only the Drupal backend. The Nuxt.js frontend should be deployed separately to a platform that supports Node.js, such as:

- Vercel
- Netlify
- Any Node.js hosting provider

See the [Lupus Decoupled documentation](https://lupus-decoupled.org) for frontend deployment details.

## Customizing the Upstream

### Adding Dependencies

To add shared dependencies that all downstream sites receive, modify `upstream-configuration/composer.json`:

```bash
composer upstream-require drupal/module_name
```

### Site-Specific Dependencies

Individual sites can add their own dependencies normally:

```bash
composer require drupal/module_name
```

## Project Structure

```
├── .ddev/                          # DDEV local development config
├── config/                         # Drupal config directory
├── upstream-configuration/         # Shared upstream dependencies
│   ├── composer.json               # Dependencies for all downstream sites
│   └── scripts/ComposerScripts.php # Pantheon upstream scripts
├── web/                            # Drupal web root
│   └── modules/custom/             # Custom modules
├── composer.json                   # Root project dependencies
├── pantheon.upstream.yml           # Pantheon platform configuration
├── phpcs.xml.dist                  # PHP CodeSniffer config
└── phpunit.xml.dist                # PHPUnit config
```

## Documentation

- [Lupus Decoupled](https://lupus-decoupled.org)
- [Pantheon Custom Upstreams](https://docs.pantheon.io/guides/custom-upstream)
- [Pantheon Integrated Composer](https://docs.pantheon.io/guides/integrated-composer)
