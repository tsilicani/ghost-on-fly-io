# Setting up a Ghost Blog on Fly.io

1. Install Fly.io CLI:
   ```bash
   curl -L https://fly.io/install.sh | sh
   ```
   or
   ```bash
   brew install flyctl
   ```

2. Create and login to Fly.io account:
   ```bash
   flyctl auth login
   ```

3. Create app:
   ```bash
   mkdir my-blog
   cd my-blog
   flyctl launch --image=ghost:5.17.2 --no-deploy
   ```

4. Create storage:
   ```bash
   flyctl volumes create data -s 3
   ```

5. Update `fly.toml`:
   ```toml
   [env]
     url = "https://your-project-name.fly.dev"
     database__client = "sqlite3"
     database__connection__filename = "content/data/ghost.db"
     database__useNullAsDefault = "true"
     database__debug = "false"
   
   [mounts]
     source="data"
     destination="/var/lib/ghost/content"

   [[services]]
     internal_port = 2368
   ```

6. Deploy:
   ```bash
   flyctl deploy
   ```

7. (Optional) Set custom domain:
   ```bash
   fly domains add hostname.com
   fly certs add hostname.com
   ```

8. (Optional) Configure transactional mailer in `fly.toml`:
   ```toml
   [env]
     mail__from = "..."
     mail__transport = "SMTP"
     mail__options__host = "..."
     mail__options__port = ...
     mail__options__auth__user = ...
     mail__options__auth__pass = ...
   ```

9. Access your blog at `https://your-project-name.fly.dev/ghost`
