# github-actions-what-inside

```bash
name: Github Action Managed Server Explore

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  explore-runner:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4

    - name: Collect system info into HTML
      run: |
        {
          echo "<!DOCTYPE html>"
          echo "<html><head><meta charset='UTF-8'>"
          echo "<title>GitHub Actions Runner Info</title>"
          echo "<style>body { font-family: monospace; background:#f9f9f9; color:#333; padding:20px; } h2 { color:#005cc5; } pre { background:#eee; padding:10px; border-radius:8px; overflow-x:auto;} details{margin-bottom:10px;}</style>"
          echo "</head><body>"
          echo "<h1>🌐 GitHub Actions Runner Exploration</h1>"
          echo "<p><b>Generated on:</b> $(date)</p>"
          echo "<hr/>"

          echo "<h2>🌍 Network / IP</h2><pre>"
          curl -s ipinfo.io
          echo "</pre>"

          echo "<h2>⚙️ System Info</h2><pre>"
          uname -a
          lsb_release -a 2>/dev/null || cat /etc/os-release
          cat /proc/version
          echo "</pre>"

          echo "<h2>⏱️ Uptime</h2><pre>"
          uptime -p
          uptime
          echo "</pre>"

          echo "<h2>💻 Hardware & Resources</h2><pre>"
          lscpu
          echo "CPU cores: $(nproc)"
          echo "</pre>"
          echo "<h2>Memory</h2><pre>"
          free -h
          echo "</pre>"

         echo "<h2>Disk Details</h2><pre>"
          df -h
          lsblk
          echo "</pre>"



          echo "<h2>📦 Installed Software</h2>"
          echo "<details><summary>dpkg -l (first 50 lines)</summary><pre>"
          dpkg -l | head -50
          echo "</pre></details>"
          echo "<pre>"
          python3 --version
          node -v
          java -version 2>&1
          go version 2>&1
          docker --version
          echo "</pre>"

          echo "<h2>📊 Processes & Monitoring</h2><pre>"
          ps aux --sort=-%mem | head -15
          echo "</pre>"



          echo "<h2>🔧 GitHub Actions Specific</h2><pre>"
          echo "Runner temp dir: $RUNNER_TEMP"
          echo "Runner tool cache: $RUNNER_TOOL_CACHE"
          echo "Job workspace: $GITHUB_WORKSPACE"
          echo "</pre>"

          echo "</body></html>"
        } > system-info.html

    - name: Upload HTML artifact
      uses: actions/upload-artifact@v4
      with:
        name: system-info
        path: system-info.html
```        