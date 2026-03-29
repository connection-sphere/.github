# Seed Configuration

In this project, the **seed configuration** is the initial catalog of 
- **channels**, 
- **profile types**, 
- **source types**, 
- **enrichment types**; and 
- **outreach types** 

that the platform needs in order to run campaigns.

In this domain, **resources** are profiles (for example LinkedIn profiles, GMail profiles, or Apollo API profiles), which are instantiated later from these seed definitions.

## Channels

A channel represents an external platform or service integration (for example LinkedIn, Facebook, Apollo, GSuite, any email verificaiton service, etc.).

Example from the seed:

```ruby
Mass::Channel.upsert({
	'name' => 'LinkedIn',
	'avatar_url' => 'file://home/leandro/code1/master/extensions/mass.commons/public/mass.commons/images/channel/linkedin.png',
	'color_code' => :light_blue
})
```

When to add one: create a new channel when you introduce a new provider that will host profiles, sources, enrichments, or outreach actions.

## Profile Types

A profile type defines how an account is used on a channel. It includes access mode and operational defaults.

Access modes used in the seed include:

- `:api` for API-based integrations,
- `:rpa` for browser automation,
- `:mta` for mail transfer accounts,
- `:basic` for custom/basic runners.

Example from the seed:

```ruby
Mass::ProfileType.upsert({
	'name' => 'LinkedIn',
	'access' => :rpa,
	'channel' => :LinkedIn,
	'available' => true,
	'description' => 'LinkedIn RPA for scraping leads and sending outreach.',
    ...
})
```

When to add one: create a new profile type when the same channel needs a different execution model or different operational defaults.

## Source Types

A source type defines how leads/companies/events are collected (searches, feeds, groups, jobs, ads, etc.).

Example from the seed:

```ruby
Mass::SourceType.upsert({
	'name' => 'Apollo_PeopleSearch',
	'description' => 'Run an Apollo search using RPA to reduce your cost by 10X.',
	'profile_type' => :Apollo_RPA,
	'available' => true,
	'default_event_limit' => 25,
	'url_pattern' => 'https://app.apollo.io/#/people',
	'paginable' => true,
    ...
})
```

When to add one: create a new source type for each new discovery workflow users can configure.

## Enrichment Types

An enrichment type defines a transformation that adds data to an existing lead/company (email verification, OSINT lookups, Apollo enrichments, etc.).

Example from the seed:

```ruby
Mass::EnrichmentType.upsert({
	'name' => 'Reoon_EmailVerification',
	'description' => 'Use Reoon API to verify the email of a lead.',
	'picture_url' => 'file://home/leandro/code1/master/extensions/mass.commons/public/mass.commons/images/enrichment-type/Apollo_LinkedInUrlToEmail.png',
	'profile_type' => :Reoon,
	'available' => true,
    ...
})
```

When to add one: create an enrichment type for each reusable enrichment workflow available in the UI.

## Outreach Types

An outreach type defines a delivery action to contact a lead (social message, connection/friend request, email via MTA).

Example from the seed:

```ruby
Mass::OutreachType.upsert({
	'name' => 'LinkedIn_ConnectionRequest',
	'description' => 'Send a LinkedIn connection request to a lead.',
	'picture_url' => 'file://home/leandro/code1/master/extensions/mass.commons/public/mass.commons/images/outreach-type/linkedin_connection_request.png',
	'profile_type' => :LinkedIn,
	'available' => true,
    ...
})
```

When to add one: create a new outreach type for each channel-specific communication method and define follow-up behavior when needed.

## Seed Manifest

The seed script loads defaults from [secret/mass/manifest.rb](secret/mass/manifest.rb):

- `MASS_API_KEY`
- `MASS_API_URL`
- `MASS_API_PORT`
- `MY_S3_URL`
- `MY_S3_API_KEY`

Command-line overrides supported by [secret/mass/seed.rb](secret/mass/seed.rb):

- `--server_url` (optional): overrides `api_url` and `api_port`.
- `--api_key` (optional): overrides any other API key source.

If `server_url` has no explicit port, Ruby `URI` infers the default by scheme (`80` for HTTP, `443` for HTTPS).

Run the script below to register the initial configuration of:

- **channels**,
- **profile types**,
- **source types**,
- **enrichment types**; and
- **outreach types**.

```bash
export RUBYLIB=~/code1/sdk && \
cd ~/code1/secret/mass && \
ruby seed.rb --server_url https://connectionsphere.com --api_key <su api key>
```

Edit [secret/mass/seed.rb](secret/mass/seed.rb) when you want to register new configurations.
  