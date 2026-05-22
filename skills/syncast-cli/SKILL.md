---
name: syncast-cli
description: Install and use Syncast CLI to generate images and videos via Syncast AI services. Use when the user asks to install Syncast CLI, log in, run imagine/video generation, or integrate Syncast into an agent harness.
---

# Syncast CLI

Syncast CLI lets external agents call Syncast's image and video generation APIs from the terminal.

## Prerequisites

- Node.js 18+ (npm/npx)
- A Syncast account

## Step 1: Install

```shell
npm install -g syncast-cli
```

Optional: install this skill for Cursor / compatible agents:

```shell
npx skills add latentcat/syncast-skills --skill syncast-cli -y
```

## Step 2: Log in

Run device authorization (opens browser or shows a link + code):

```shell
syncast auth login
```

For a custom API endpoint:

```shell
syncast auth login --api-url https://your-api.example.com
```

## Step 3: Verify

```shell
syncast auth status
```

Expected: `logged_in: true` with your username.

## Step 4: Generate

### Image

```shell
syncast imagine --prompt "a cat on the moon" --model nano-banana-pro --aspect-ratio 16:9
```

### Video

```shell
syncast video --prompt "a horse running on the beach" --model veo-3-1 --duration 8
```

### Human-readable output

```shell
syncast imagine --prompt "sunset over mountains" --format human
```

Default output is JSON (for agents).

## Task management

```shell
# Stream / wait for an existing task
syncast task status <task_id>

# Cancel
syncast task cancel <task_id>
```

## Local file sync (legacy)

```shell
syncast sync
# or
syncast start
```

## Common models

| Media | Models |
|-------|--------|
| Image | `nano-banana-pro`, `nano-banana-2`, `seedream-4-5`, `gpt-image-2` |
| Video | `veo-3-1`, `sora-2`, `seedance2.0pro`, `grok-video-3` |

## Troubleshooting

| Issue | Action |
|-------|--------|
| Not logged in | `syncast auth login` |
| Session expired | `syncast auth logout` then login again |
| 402 insufficient credits | Add credits in Syncast dashboard |
| Device code expired | Re-run `syncast auth login` |

## Configuration

Credentials: `~/.syncast/config.json`

Environment:

- `SYNCAST_API_URL` or `API_URL` — API base URL (default `https://dev-syncast-service.latentnet.com`)

## Agent workflow summary

1. `syncast auth login` — user completes browser authorization
2. `syncast auth status` — confirm login
3. `syncast imagine` or `syncast video` — parse JSON output for `image_url` / `video_url`
4. Use URLs in downstream agent steps
