<p align="center">
  <a href="https://www.loudly.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="assets/loudly-logo-white.png">
      <img alt="Loudly" src="assets/loudly-logo-black.png" width="220">
    </picture>
  </a>
</p>

<h1 align="center">Loudly MCP server</h1>

<p align="center">Make, edit, and master music with Loudly right from your AI assistant.</p>

<p align="center">
  <a href="https://registry.modelcontextprotocol.io/?q=loudly"><img alt="MCP Registry" src="https://img.shields.io/badge/MCP%20Registry-io.github.Loudly--AI%2Floudly-2ea44f"></a>
  <img alt="Transport" src="https://img.shields.io/badge/transport-Streamable%20HTTP-blue">
  <img alt="Auth" src="https://img.shields.io/badge/auth-OAuth%20with%20PKCE-blue">
  <a href="https://www.loudly.com/mcp/docs"><img alt="Docs" src="https://img.shields.io/badge/docs-loudly.com%2Fmcp%2Fdocs-lightgrey"></a>
  <a href="https://glama.ai/mcp/connectors/io.github.Loudly-AI/loudly"><img alt="Loudly MCP connector: tool definition quality and endpoint health on Glama" src="https://glama.ai/mcp/connectors/io.github.Loudly-AI/loudly/badges/score.svg"></a>
</p>

<p align="center">
  <a href="https://www.loudly.com/mcp">Product page</a> ·
  <a href="https://www.loudly.com/mcp/docs">Setup docs</a> ·
  <a href="https://www.loudly.com/ai-music-agent">AI Music Agent</a> ·
  <a href="https://www.loudly.com/music/pricing">Pricing</a> ·
  <a href="mailto:help@loudly.com">Support</a>
</p>

The Loudly MCP server lives at **https://mcp.loudly.com**. Connect it to an assistant and ask in plain words: "write a lo-fi track with soft female vocals", "split this song into stems", "master it like a modern pop record". The assistant picks the right Loudly tools, runs them, and hands you the files.

Loudly does the music work on its own servers: generation, stem separation, mixing, mastering, and audio editing. The assistant turns your request into tool calls and keeps track of the results. The same tools power the [Loudly AI Music Agent](https://www.loudly.com/ai-music-agent), Loudly's own chat, if you would rather not set up a client.

You need a Loudly account, and a free one is enough to start: it comes with a capped credit allowance so you can try the tools for non-commercial use. Every tool call that does real work spends credits. Personal and Pro plans include a monthly allowance that refreshes every month, a commercial license, and the option to buy add-on credits when you run low; see the [pricing page](https://www.loudly.com/music/pricing) for the current numbers. You manage your plan and credits in the Loudly Webapp.

## Add it to an assistant

You sign in with your Loudly account. There is no API key to copy.

### Claude

1. Open **Customize**, then **Connectors**.
2. Click **+ Add**, then **Add custom connector**.
3. Enter the name `Loudly`, paste `https://mcp.loudly.com` and click **Continue**.
4. Choose **Sign in now**, click **Add** and sign in with Loudly.

On Team and Enterprise plans, an owner adds the connector first in Organization settings, then each member clicks **Connect**.

### ChatGPT

1. Open **Plugins** in ChatGPT on the web.
2. Click the **+** button, then **Add custom MCP server**.
3. Enter the name `Loudly` and paste `https://mcp.loudly.com` under Connection.
4. Choose **OAuth**, confirm the warning, click **Create as a plugin** and sign in with Loudly.
5. Install the plugin. In a chat, type `@` and select Loudly.

Works on ChatGPT web with a Business, Enterprise or Edu workspace, where an admin may need to add it. Pro accounts can connect it with read-only access.

### Claude Code

```bash
claude mcp add --transport http loudly https://mcp.loudly.com
```

Then run `/mcp` inside a session to sign in.

### Cursor

Add this to `.cursor/mcp.json`, or to `~/.cursor/mcp.json` for all projects:

```json
{
  "mcpServers": {
    "loudly": { "url": "https://mcp.loudly.com" }
  }
}
```

### VS Code

Run **MCP: Add Server** from the command palette, choose **HTTP**, and paste `https://mcp.loudly.com`. Or add this to `.mcp.json` in your workspace root:

```json
{
  "mcpServers": {
    "loudly": { "type": "http", "url": "https://mcp.loudly.com" }
  }
}
```

### Other clients

Any client that supports remote MCP servers with OAuth works. Add `https://mcp.loudly.com` as a remote server.

## What it can do

Ask for what you want to hear or change. The assistant chains the tools for you: upload a song, separate its stems, remix them, master the result, and export a WAV, all in one conversation.

| Tool | What it does | Creates |
| --- | --- | --- |
| `get_credit_balance` | Shows the credits you can still spend. | nothing |
| `load_skill` | Loads the instructions of a Loudly skill before a task it covers. | nothing |
| `request_audio_upload` | Shows you a panel to pick one audio file from your device. | audio |
| `create_audio_upload_link` | Gives the assistant a URL to POST one audio file to, without your browser. | audio |
| `ingest_audio_from_url` | Fetches audio from a URL that serves the file directly. | audio |
| `put_artifact` | Uploads a small audio file or a project XML inline. | audio or project |
| `get_artifact` | Looks up an artifact and returns its details and download URL. | nothing |
| `generate_with_manta` | Writes a full song with sung vocals from a style prompt and optional lyrics, or an instrumental. | audio |
| `generate_with_stable_audio` | Makes up to 360 s of instrumental music, sound effects or textures from a prompt. | audio |
| `generate_with_vega` | Builds an instrumental project out of Loudly library loops in a chosen genre. | project |
| `get_vega_genres` | Lists the VEGA genres with description and bpm range. | nothing |
| `render_vega_project` | Renders a project to audio. | audio |
| `restyle_with_manta` | Covers a sung song: same melody and lyrics, new arrangement and singer. | audio |
| `restyle_with_stable_audio` | Keeps the original vocal and regenerates everything under it in a new style. | audio |
| `vary_with_stable_audio` | Makes alternative takes of an instrumental, same style and structure. | audio |
| `extend_with_stable_audio` | Continues a track past its end, keeping the original at the start. | audio |
| `prepare_remix_with_vega` | Splits a song into stems and loops per section and instrument: a remix pack. | remix pack |
| `remix_with_vega` | Keeps a song's stems and arranges Loudly library loops around them in a new genre. | project |
| `analyze_song_dna` | Reads mood, key, bpm, instruments, genres and loudness. | nothing |
| `analyze_song_structure_and_lyrics` | Adds lyrics and a section map to the DNA. Takes minutes. | nothing |
| `loudness_meter` | Measures integrated loudness (LUFS), loudness range (LU), sample peak and true peak. | nothing |
| `search_catalog_songs` | Finds Loudly catalog songs by description, by metadata filters, or both. | nothing |
| `search_loops` | Finds the 3 Loudly library loops closest to a description. | nothing |
| `search_mastering_references` | Finds mastering references by prompt, genre, and numeric ranges. | nothing |
| `separate_audio_stems` | Splits an audio file into stems keyed by instrument. | audio |
| `separate_vega_project_stems` | Splits a project into one single instrument project per stem. | project |
| `run_mixing_workflow` | An AI mixing engineer balances the stems of one song and explains its settings. | audio |
| `run_mastering_workflow` | Masters a track to a reference, a catalog reference, or a genre, then limits it. | audio |
| `trim_audio` | Cuts audio to a start and end time, with click-free micro fades. | audio |
| `stitch_audio_files` | Joins several clips end to end in order. | audio |
| `combine_audio` | Places several clips on one timeline with start time and gain, and mixes them. | audio |
| `add_fades` | Adds a fade in, a fade out, or both. | audio |
| `gain_audio` | Raises or lowers the level by a fixed number of dB. | audio |
| `equalize_audio` | Runs audio through a chain of low-pass, high-pass, peak and shelf filters. | audio |
| `compress_audio` | Compresses dynamics above a threshold. | audio |
| `limit_audio` | Holds peaks under a ceiling with a lookahead limiter. | audio |
| `warp_audio` | Time stretches and pitch shifts, each independent of the other. | audio |
| `reverse_audio` | Plays audio backwards. | audio |
| `bitcrush_audio` | Lowers bit depth and sample rate for a lo-fi, aliased sound. | audio |
| `paulstretch_audio` | Stretches audio into an ambient, transient-free texture. | audio |
| `export_audio` | Converts audio to another format, sample rate, bitrate, or bit depth for download. | download |
| `bundle_artifacts` | Zips several artifacts into one download, such as stems or a remix pack. | download |

Tools that create something never overwrite their input. Each edit returns a new artifact, so earlier versions stay available in the conversation.

## Long jobs

Most tools answer within the call. Generation, restyling, stem separation, remix preparation, and full song analysis can take longer. These tools wait up to 100 seconds. If the job is still queued or running by then, they return a `job_id` and the assistant collects the result with the matching wait tool:

| Started by | Collected with |
| --- | --- |
| `generate_with_manta` | `wait_for_generate_with_manta` |
| `restyle_with_manta` | `wait_for_restyle_with_manta` |
| `generate_with_stable_audio` | `wait_for_generate_with_stable_audio` |
| `restyle_with_stable_audio` | `wait_for_restyle_with_stable_audio` |
| `vary_with_stable_audio` | `wait_for_vary_with_stable_audio` |
| `extend_with_stable_audio` | `wait_for_extend_with_stable_audio` |
| `prepare_remix_with_vega` | `wait_for_prepare_remix_with_vega` |
| `separate_audio_stems` | `wait_for_separate_audio_stems` |
| `analyze_song_dna` | `wait_for_analyze_song_dna` |
| `analyze_song_structure_and_lyrics` | `wait_for_analyze_song_structure_and_lyrics` |

Each wait tool blocks up to 100 seconds. The assistant calls it again with the same `job_id` while the status is `queued` or `running`. The statuses `done` and `failed` are final.

## Artifacts

Everything the server works on is an artifact with an `artifact_id`. Artifacts expire after 2 days, so export anything you want to keep. There are two kinds:

| Kind | What it is |
| --- | --- |
| audio | A lossless master plus an mp3 preview. |
| project | A VEGA arrangement of Loudly library loops, stored as XML. Render it with `render_vega_project` to hear it. |

There are four ways to bring your own audio in:

- **From you:** `request_audio_upload` shows an upload panel in the chat. The artifact exists once the panel reports the upload is done. One file per panel.
- **From the assistant:** `create_audio_upload_link` returns a URL. A harness that can make HTTP requests POSTs the raw bytes to it, optionally with `?name=<file name>`, and gets the finished artifact back in one request.
- **From a link:** `ingest_audio_from_url` fetches a URL that serves the audio bytes directly, not a player or share page.
- **Inline:** `put_artifact` takes a small file as base64, or project XML as text.

To take audio out, ask for a format. `export_audio` returns a download URL in mp3, ogg, aac, m4a, opus, wav, aiff, or flac. Bitrate applies to lossy formats, from 32 to 512 kbps. Sample rate can be set from 8000 to 192000 Hz. Bit depth applies to lossless formats: 16, 24, or 32-bit float for WAV and AIFF, 16 or 24 for FLAC. Opus supports sample rates of 8000, 12000, 16000, 24000 and 48000 Hz only. For several files at once, `bundle_artifacts` returns one zip.

## Credits

Tool calls are billed in Loudly credits. A free account starts with a capped allowance that cannot be topped up; Personal and Pro plans refresh their allowance monthly and can buy add-on credits. Priced tools report their cost and your balance after each call, and `get_credit_balance` shows what is left at any time. When you run out, the server tells the assistant: on a free account, it points you to the plans; on a paid plan, to the top-up page in the Loudly Webapp. To start or change a plan, go to the [pricing page](https://www.loudly.com/music/pricing).

## Skills

Skills are instructions that teach the assistant how to run a Loudly task well. The assistant loads one with `load_skill` before a task it covers. The list of available skills is part of the tool's description, so the assistant always sees the current set.

## Sign-in for apps

The server uses OAuth with your Loudly account. Clients that support MCP sign in find everything below on their own, starting from the server's metadata.

| Endpoint | URL |
| --- | --- |
| Protected resource metadata | https://mcp.loudly.com/.well-known/oauth-protected-resource |
| Authorization server | https://www.loudly.com |
| Authorization server metadata | https://www.loudly.com/.well-known/oauth-authorization-server |
| Where sign-in starts | https://www.loudly.com/oauth/v2/authorize |
| Exchanges a code for tokens | https://www.loudly.com/oauth/v2/token |
| Registers a new client | https://www.loudly.com/oauth/v2/register |

Sign in uses the authorization code flow with PKCE (S256). Clients register themselves through dynamic client registration and can be public clients with no secret, or send a secret with `client_secret_post`. Refresh tokens are supported, so you stay signed in. The access token goes in the `Authorization: Bearer` header of each request.

There is one scope, `mcp`, which allows you to use all the tools of the Loudly MCP server with your account and credits.

## Registry

This server is listed in the official MCP Registry as `io.github.Loudly-AI/loudly`. The `server.json` in this repository is the published record.

- https://registry.modelcontextprotocol.io/?q=loudly

## Links

**Loudly MCP**

- Product page: https://www.loudly.com/mcp
- Setup docs: https://www.loudly.com/mcp/docs
- Official MCP Registry listing: https://registry.modelcontextprotocol.io/?q=loudly
- Support: help@loudly.com

**Loudly**

- Website: https://www.loudly.com
- AI Music Agent, create, remix and master in one chat: https://www.loudly.com/ai-music-agent
- Pricing: https://www.loudly.com/music/pricing
- Knowledge base: https://www.loudly.com/knowledge-base
- Blog: https://www.loudly.com/blog

**Developers**

- Music API: https://www.loudly.com/music-api
- JavaScript SDK: https://github.com/Loudly-AI/loudly-js-sdk
- GitHub organization: https://github.com/Loudly-AI

**More Loudly tools**

- Stem Splitter: https://www.loudly.com/stem-splitter
- AI Mastering: https://www.loudly.com/ai-song-mastering
- AI Song Remixer: https://www.loudly.com/ai-song-remixer
- AI Sample Generator: https://www.loudly.com/ai-sample-generator
- Music Distribution: https://www.loudly.com/music-distribution

**Follow**

- Instagram: https://www.instagram.com/getloudly/
- YouTube: https://www.youtube.com/channel/UCxgEFs7vl72VYTClAFxvR3g

## Assets

Logo files for client galleries and directories are in [`assets/`](assets/): `loudly-logo-black.png` (800×300, for light backgrounds), `loudly-logo-white.png` (2000×750, for dark backgrounds), and `loudly-icon-192.png` (square icon, the one used in the registry listing).

## License

The contents of this repository, the README, `server.json` and the files under `assets/` are released under the [MIT License](LICENSE). Copyright (c) 2026 Loudly GmbH.

The Loudly name and logos are trademarks of Loudly GmbH. The license above does not grant permission to use them except to identify the Loudly MCP server, for example in a client's server gallery. The Loudly MCP server itself, the music it produces, and your account are governed by Loudly's [terms and conditions](https://www.loudly.com/terms-and-conditions) and [license agreement](https://www.loudly.com/license-agreement).
