[![GitGrowBot Follower (Scheduled hourly)](https://github.com/ikramagix/GitGrowBot/actions/workflows/run_follow.yml/badge.svg)](https://github.com/ikramagix/GitGrowBot/actions/workflows/run_follow.yml)
[![GitGrowBot Unfollower (Scheduled weekly)](https://github.com/ikramagix/GitGrowBot/actions/workflows/run_unfollow.yml/badge.svg)](https://github.com/ikramagix/GitGrowBot/actions/workflows/run_unfollow.yml)

# GitGrowBot 

GitGrowBot is your personal GitHub networking assistant. It's an automation tool designed to help you **grow** and **nurture** your developer network organically. With GitGrowBot, you’ll:

* **Follow** users from our curated list, up to a configurable limit per run.
* **Unfollow** anyone who doesn’t follow you back, because **reciprocity** matters.
* (COMING SOON) **Star** and **unstar** repositories with the same give-and-take logic.

All actions run on a schedule (or on demand) in GitHub Actions, so you never need to manually review your follow list. 

## How it works
The motto **“You only get what you give”** drives GitGrowBot’s behavior:

1. GitGrowBot **follow** someone for you—chances are, they’ll notice and **follow you back** (especially if they’re clever like you and use GitGrowBot too!).  
2. If they **don’t** reciprocate by the next run, GitGrowBot quietly **unfollows** them.
3. Soon, we’ll extend this to **stars**: you star their repo, they star yours; you unstar, GitGrowBot unstars theirs.

This ensures your follow list stays active while you're busy coding.

## Features

- **Automated Follow / Unfollow**  
  - Follow 100 fresh users each run.  
  - Unfollow non-reciprocals.  
  - Skip any usernames you whitelist.  
- **Cleaner utility** (`scripts/cleaner.py`)  
  - Deduplicates and prunes dead GitHub usernames locally.  
- **Offline logging**  
  - Records missing usernames in `logs/offline_usernames-<timestamp>.txt`.  
- **CI-first, dev-friendly**  
  - Runs hands-free in Actions.  
  - `.env` support for local testing (optional).  
- **Modular code**  
  - `scripts/gitgrow.py` for main logic.  
  - `scripts/cleaner.py` for list maintenance.  
- **Prebuilt Workflow**  
  - `.github/workflows/run_follow.yml`: Runs **every hour at minute 5** (UTC) by default.
  - `.github/workflows/run_unfollow.yml` runs **every 10 hours at minute 5** (UTC) by default.
  - `.github/workflows/manual_follow.yml` – manual trigger: **follow & follow-back only**  
  - `.github/workflows/manual_unfollow.yml` – manual trigger: **unfollow non-reciprocals only**  

## Getting started

1. **Fork** or **clone** this repo.
2. In **Settings → Secrets → Actions**, add your Github PAT as `PAT_TOKEN` (scope: `user:follow`).
3. In **Settings → Variables → Repository variables**, add **`BOT_USER`** with _your_ GitHub username. *This prevents the workflow from running in other people’s forks unless they set their own name.*
4. **10K+ members like you who want to grow are waiting for you in** `config/usernames.txt`.  
You can join this list too—see below (**⭐ Don't miss out: Join our 10000+ users**).
5. (Optional) Tweak the schedules in your workflow files:
    - `.github/workflows/run_follow.yml` runs **hourly at minute 5** by default.
    - `.github/workflows/run_unfollow.yml` runs **every 10 hours at minute 5** (UTC) by default.
6. (Important) Edit `config/whitelist.txt` to protect any accounts you never want the script to act on (no unfollowing, no unstarring for usernames in `whitelist.txt`).
7. (Optional) Copy `.env.example` → `.env` for local testing (or contributors).
8. **Enable** GitHub Actions in your repo settings.
9. Sit back and code—**GitGrowBot** does the networking for you!  

## Local testing

If you want to test the bot locally, you can use the provided `scripts/cleaner.py` and `scripts/gitgrow.py` scripts.

1. Copy `.env.example` → `.env` and fill in your PAT.
2. Run the following commands:

```bash
# Example local run of cleanup
python scripts/cleaner.py

# Example local dry-run of follow bot
python scripts/gitgrow.py
```

## ⭐ Don't miss out: Join our 10000+ users!

Want in? It’s effortless. If you:

1. **Star** this repository, **AND**
2. **Follow** both **[@ikramagix](https://github.com/ikramagix)** and **[@gr33kurious](https://github.com/gr33kurious)**

then your username will be **automatically** added to the master `usernames.txt` list alongside the **10K+** active members!

Let's grow! 💪

## Configuration

| Options      | Description                                                   | Default                                             |
| ------------------ | ------------------------------------------------------------- | --------------------------------------------------- |
| PAT_TOKEN       | Your PAT with `user:follow` scope, added in your secrets   |(empty) **required**                                      |
| USERNAME_FILE      | File listing target usernames (in the `config/` directory)  | `config/usernames.txt` (keep it as it is)            |
| WHITELIST_FILE     | File listing usernames never to unfollow (in `config/`)          | `config/whitelist.txt` (editable, add usernames) |
| FOLLOWERS_PER_RUN  | How many new users to follow each run                         | `100` (keep it low to avoid rate-limit)               |

## Repository structure

```
├── .gitattributes
├── .github
│   └── workflows
│       ├── run_follow.yml       # Scheduled: follow-only (hourly @ :05)
│       ├── run_unfollow.yml     # Scheduled: unfollow-only (daily every 10 hours @ :05 UTC)
│       ├── manual_follow.yml    # workflow_dispatch → follow only
│       └── manual_unfollow.yml  # workflow_dispatch → unfollow only
├── .gitignore
├── README.md
├── config
│   ├── usernames.txt            # 5,500+ community members
│   └── whitelist.txt            # accounts to always skip
├── logs                         # CI artifacts (gitignored)
│   └── offline_usernames-*.txt
├── requirements.txt
└── scripts
│   ├── gitgrow.py               # Main follow/unfollow driver
│   ├── unfollowers.py           # Unfollow-only logic
│   └── cleaner.py               # Username list maintenance
├──tests/
│   ├── test_bot_core_behavior.py   # follow/unfollow/follow-back
│   ├── test_unfollowers.py         # unfollow-only logic
│   └── test_cleaner.py             # cleaner dedupe + missing-user removal
```
### Manual Troubleshooting Runners (optional)

If you ever need to isolate one step for debugging, head to your repo’s **Actions** tab:

- **GitGrowBot Manual Follow** (`.github/workflows/manual_follow.yml`)  
  Manually triggers **only** the follow & follow-back logic.  
- **GitGrowBot Manual Unfollow** (`.github/workflows/manual_unfollow.yml`)  
  Manually triggers **only** the unfollow non-reciprocals logic.  

Choose the workflow, click **Run workflow**, select your branch, and go!

## Contributing

We started building GitGrowBot as a peer-to-peer coding challenge on a sleepless night. But it doesn't have to end here. Feel free to:

1. **Open an issue** to suggest new features, report bugs, or share ideas.  
2. **Submit a pull request** to add enhancements, fix problems, or improve documentation.  
3. Join the discussion—your use cases, feedback, and code all keep our community vibrant.

Every contribution, big or small, helps everyone grow. Thank you for pitching in!

**Happy networking & happy coding!** 
_And thank you for saying thank you! If you find this project useful, please consider giving it a star or supporting us on **buymeacoffee** below._

[![Buy Me A Coffee](https://img.shields.io/badge/Buy_Me_A_Coffee-FFDD00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://www.buymeacoffee.com/ikramagix)

<div>
<a href="https://www.buymeacoffee.com/ikramagix" target="_blank">
  <img 
    src="https://i.ibb.co/tP37SFx/cuphead-thx-nobg.png" 
    alt="Buy Me A Coffee" 
    width="150">
</a>
</div>

<br>

– With 💛 from [@ikramagix](https://github.com/ikramagix) & [@gr33kurious](https://github.com/gr33kurious)
