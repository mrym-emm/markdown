---


---

<h1 id="complete-windows-setup-guide-for-ngam-je-backend-postgresql--pgadmin--fastapi">Complete Windows Setup Guide for Ngam-Je Backend (PostgreSQL + pgAdmin + FastAPI)</h1>
<h2 id="prerequisites">Prerequisites</h2>
<ul>
<li>
<p>Windows 10/11</p>
</li>
<li>
<p>Admin access to your computer</p>
</li>
</ul>
<h2 id="step-1-install-docker-desktop">Step 1: Install Docker Desktop</h2>
<ol>
<li>
<p>Download from: <a href="https://www.docker.com/products/docker-desktop/">https://www.docker.com/products/docker-desktop/</a></p>
</li>
<li>
<p>Install and restart your computer if prompted</p>
</li>
<li>
<p>Open Docker Desktop and make sure it’s running (whale icon in system tray)</p>
</li>
</ol>
<h2 id="step-2-install-wsl-windows-subsystem-for-linux">Step 2: Install WSL (Windows Subsystem for Linux)</h2>
<p>WSL solves Windows networking issues with Docker and gives you a Linux environment like your teacher uses.</p>
<h3 id="installation">Installation:</h3>
<ol>
<li>
<p>Open PowerShell as Administrator (right-click → Run as Administrator)</p>
</li>
<li>
<p>Run:</p>
</li>
</ol>
<pre class=" language-powershell"><code class="prism  language-powershell">
wsl <span class="token operator">--</span>install

</code></pre>
<ol start="3">
<li>
<p>Restart your computer</p>
</li>
<li>
<p>After restart, Ubuntu will open automatically. Create:</p>
</li>
</ol>
<ul>
<li>
<p>Username (can be anything)</p>
</li>
<li>
<p>Password (remember this!)</p>
</li>
</ul>
<h3 id="verify-wsl-is-working">Verify WSL is working:</h3>
<pre class=" language-powershell"><code class="prism  language-powershell">
wsl <span class="token operator">--</span>status

</code></pre>
<p>You should see:</p>
<pre><code>
Default Distribution: Ubuntu

Default Version: 2

</code></pre>
<h2 id="step-3-fix-.env-file-important">Step 3: Fix .env File (IMPORTANT!)</h2>
<p>The .env file needs to be in the backend folder with the correct email format.</p>
<h3 id="location">Location:</h3>
<p>Move .env from project root to backend folder:</p>
<pre class=" language-powershell"><code class="prism  language-powershell">
cd C:\Users\<span class="token namespace">[YOUR-USERNAME]</span>\path\to\ngam<span class="token operator">-</span>je

<span class="token function">move</span> <span class="token punctuation">.</span>env backend\

</code></pre>
<h3 id="fix-the-pgadmin-email">Fix the pgAdmin email:</h3>
<p>Open <code>backend\.env</code> and change:</p>
<pre><code>
PGADMIN_EMAIL=admin@ngamje.local

</code></pre>
<p>To:</p>
<pre><code>
PGADMIN_EMAIL=admin@example.com

</code></pre>
<p><strong>Why?</strong> Windows pgAdmin rejects <code>.local</code> domains - this is a known issue.</p>
<h2 id="step-4-start-docker-services">Step 4: Start Docker Services</h2>
<p>Open PowerShell or VSCode terminal in the project root:</p>
<pre class=" language-powershell"><code class="prism  language-powershell">
cd C:\Users\<span class="token namespace">[YOUR-USERNAME]</span>\path\to\ngam<span class="token operator">-</span>je

docker<span class="token operator">-</span>compose up <span class="token operator">-</span>d postgres pgadmin

</code></pre>
<h3 id="check-if-running">Check if running:</h3>
<pre class=" language-powershell"><code class="prism  language-powershell">
docker<span class="token operator">-</span>compose <span class="token function">ps</span>

</code></pre>
<p>You should see both <code>ngam-je-postgres</code> and <code>ngam-je-pgadmin</code> with status “Up”.</p>
<h2 id="step-5-access-pgadmin">Step 5: Access pgAdmin</h2>
<ol>
<li>
<p>Open browser: <a href="http://localhost:5050">http://localhost:5050</a></p>
</li>
<li>
<p>Login is automatic (no password needed due to dev config)</p>
</li>
<li>
<p>Add your database server:</p>
</li>
</ol>
<ul>
<li>Right-click “Servers” → “Register” → “Server”</li>
</ul>
<p><strong>General tab:</strong></p>
<ul>
<li>Name: <code>Ngam-je Local</code></li>
</ul>
<p><strong>Connection tab:</strong></p>
<ul>
<li>
<p>Host: <code>postgres</code> (Docker container name, NOT localhost)</p>
</li>
<li>
<p>Port: <code>5432</code></p>
</li>
<li>
<p>Database: <code>ngamje_db</code></p>
</li>
<li>
<p>Username: <code>ngamje</code></p>
</li>
<li>
<p>Password: <code>ngamje_dev_password</code></p>
</li>
</ul>
<ol start="4">
<li>Click “Save”</li>
</ol>
<h2 id="step-6-set-up-backend-in-wsl">Step 6: Set Up Backend in WSL</h2>
<h3 id="enter-wsl">Enter WSL:</h3>
<p>In VSCode terminal or PowerShell:</p>
<pre class=" language-powershell"><code class="prism  language-powershell">
wsl

</code></pre>
<p>You’ll see prompt change to: <code>yourname@computername:~$</code></p>
<h3 id="navigate-to-project">Navigate to project:</h3>
<pre class=" language-bash"><code class="prism  language-bash">
<span class="token function">cd</span> /mnt/c/Users/<span class="token punctuation">[</span>YOUR-USERNAME<span class="token punctuation">]</span>/path/to/ngam-je/backend

</code></pre>
<p><strong>Example:</strong></p>
<pre class=" language-bash"><code class="prism  language-bash">
<span class="token function">cd</span> /mnt/c/Users/marya/OneDrive/Desktop/capstone_final/ngam-je/backend

</code></pre>
<h3 id="verify-.env-file-is-there">Verify .env file is there:</h3>
<pre class=" language-bash"><code class="prism  language-bash">
<span class="token function">ls</span> -la <span class="token operator">|</span> <span class="token function">grep</span> .env

</code></pre>
<p>You should see:</p>
<pre><code>
-rwxrwxrwx 1 user user 1090 Nov 1 07:41 .env

</code></pre>
<h2 id="step-7-install-uv-python-package-manager">Step 7: Install uv (Python Package Manager)</h2>
<pre class=" language-bash"><code class="prism  language-bash">
<span class="token comment"># Install uv</span>

curl -LsSf https://astral.sh/uv/install.sh <span class="token operator">|</span> sh

  

<span class="token comment"># Add to PATH</span>

<span class="token function">export</span> PATH<span class="token operator">=</span><span class="token string">"<span class="token variable">$HOME</span>/.local/bin:<span class="token variable">$PATH</span>"</span>

  

<span class="token comment"># Verify installation</span>

uv --version

</code></pre>
<p>You should see: <code>uv 0.9.7</code> (or similar)</p>
<h2 id="step-8-install-python-dependencies">Step 8: Install Python Dependencies</h2>
<pre class=" language-bash"><code class="prism  language-bash">
uv <span class="token function">sync</span>

</code></pre>
<p><strong>Note:</strong> You might see a warning about hardlinking - this is normal on WSL and doesn’t affect functionality.</p>
<h2 id="step-9-run-database-migrations">Step 9: Run Database Migrations</h2>
<pre class=" language-bash"><code class="prism  language-bash">
uv run alembic upgrade <span class="token function">head</span>

</code></pre>
<h3 id="expected-output">Expected output:</h3>
<pre><code>
INFO [alembic.runtime.migration] Context impl PostgresqlImpl.

INFO [alembic.runtime.migration] Will assume transactional DDL.

INFO [alembic.runtime.migration] Running upgrade -&gt; 143637ed0a88, Add User model

</code></pre>
<h3 id="verify-tables-were-created">Verify tables were created:</h3>
<pre class=" language-bash"><code class="prism  language-bash">
docker <span class="token function">exec</span> -it ngam-je-postgres psql -U ngamje -d ngamje_db -c <span class="token string">"\dt"</span>

</code></pre>
<p>You should see:</p>
<pre><code>
Schema | Name | Type | Owner

--------+-----------------+-------+--------

public | alembic_version | table | ngamje

public | users | table | ngamje

</code></pre>
<h2 id="step-10-start-the-backend-server">Step 10: Start the Backend Server</h2>
<pre class=" language-bash"><code class="prism  language-bash">
uv run uvicorn src.app.main:app --reload --host 0.0.0.0 --port 8000

</code></pre>
<h3 id="expected-output-1">Expected output:</h3>
<pre><code>
INFO: Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)

INFO: Application startup complete.

</code></pre>
<h3 id="access-the-api">Access the API:</h3>
<ul>
<li>
<p><strong>API Root:</strong> <a href="http://localhost:8000">http://localhost:8000</a></p>
</li>
<li>
<p><strong>API Docs (Swagger):</strong> <a href="http://localhost:8000/docs">http://localhost:8000/docs</a></p>
</li>
<li>
<p><strong>Health Check:</strong> <a href="http://localhost:8000/api/v1/health">http://localhost:8000/api/v1/health</a></p>
</li>
</ul>
<p>The terminal will show logs as requests come in - this is normal!</p>
<hr>
<h2 id="common-errors--solutions">Common Errors &amp; Solutions</h2>
<h3 id="error-password-authentication-failed-for-user-ngamje">Error: password authentication failed for user “ngamje”</h3>
<p><strong>Cause:</strong> Windows networking issues connecting from Python to Docker PostgreSQL.</p>
<p><strong>Solution:</strong> Use WSL (Steps 2 &amp; 6-10 above). WSL provides Linux networking that works seamlessly with Docker.</p>
<h3 id="error-no-pyvenv.cfg-file-with-exit-code-106">Error: No pyvenv.cfg file with exit code 106</h3>
<p><strong>Cause:</strong> Broken virtual environment.</p>
<p><strong>Solution:</strong> Delete and recreate:</p>
<pre class=" language-bash"><code class="prism  language-bash">
<span class="token function">rm</span> -r .venv

uv <span class="token function">sync</span>

</code></pre>
<h3 id="error-pgadmin-keeps-restarting">Error: pgAdmin keeps restarting</h3>
<p><strong>Cause:</strong> Invalid email format in .env.</p>
<p><strong>Solution:</strong> Change <code>PGADMIN_EMAIL</code> from <code>admin@ngamje.local</code> to <code>admin@example.com</code> (Step 3).</p>
<h3 id="error-cant-activate-virtual-environment-in-wsl">Error: Can’t activate virtual environment in WSL</h3>
<p>Don’t try to activate manually! Commands like:</p>
<pre class=" language-bash"><code class="prism  language-bash">
<span class="token function">source</span> .venv/Scripts/activate <span class="token comment"># ❌ WRONG - Windows path</span>

</code></pre>
<p>Instead: Use <code>uv run</code> which handles activation automatically:</p>
<pre class=" language-bash"><code class="prism  language-bash">
uv run alembic upgrade <span class="token function">head</span> <span class="token comment"># ✅ CORRECT</span>

uv run uvicorn src.app.main:app --reload <span class="token comment"># ✅ CORRECT</span>

</code></pre>
<h3 id="error-uv-command-not-found">Error: uv: command not found</h3>
<p><strong>Solution:</strong> Add to PATH:</p>
<pre class=" language-bash"><code class="prism  language-bash">
<span class="token function">export</span> PATH<span class="token operator">=</span><span class="token string">"<span class="token variable">$HOME</span>/.local/bin:<span class="token variable">$PATH</span>"</span>

</code></pre>
<p>To make permanent, add to <code>~/.bashrc</code>:</p>
<pre class=" language-bash"><code class="prism  language-bash">
<span class="token keyword">echo</span> <span class="token string">'export PATH="<span class="token variable">$HOME</span>/.local/bin:<span class="token variable">$PATH</span>"'</span> <span class="token operator">&gt;&gt;</span> ~/.bashrc

<span class="token function">source</span> ~/.bashrc

</code></pre>
<h3 id="error-fastapi-dev-doesnt-work">Error: fastapi dev doesn’t work</h3>
<p><strong>Solution:</strong> Use uvicorn directly instead:</p>
<pre class=" language-bash"><code class="prism  language-bash">
uv run uvicorn src.app.main:app --reload --host 0.0.0.0 --port 8000

</code></pre>
<hr>
<h2 id="using-vscode-with-wsl">Using VSCode with WSL</h2>
<h3 id="option-1-wsl-terminal-in-vscode">Option 1: WSL Terminal in VSCode</h3>
<ol>
<li>
<p>Press `Ctrl + `` (backtick) to open terminal</p>
</li>
<li>
<p>Click dropdown next to <code>+</code> icon</p>
</li>
<li>
<p>Select “Ubuntu (WSL)”</p>
</li>
</ol>
<h3 id="option-2-connect-vscode-to-wsl-recommended">Option 2: Connect VSCode to WSL (Recommended)</h3>
<ol>
<li>
<p>Install “WSL” extension in VSCode</p>
</li>
<li>
<p>Press <code>Ctrl + Shift + P</code></p>
</li>
<li>
<p>Type “WSL: Connect to WSL”</p>
</li>
<li>
<p>VSCode reloads and runs entirely in WSL!</p>
</li>
<li>
<p>Open folder: <code>/mnt/c/Users/[YOUR-USERNAME]/path/to/project</code></p>
</li>
</ol>
<hr>
<h2 id="quick-reference-commands">Quick Reference Commands</h2>
<h3 id="start-everything">Start everything:</h3>
<pre class=" language-powershell"><code class="prism  language-powershell">
<span class="token comment"># From project root (Windows PowerShell)</span>

cd C:\Users\<span class="token namespace">[YOUR-USERNAME]</span>\path\to\ngam<span class="token operator">-</span>je

docker<span class="token operator">-</span>compose up <span class="token operator">-</span>d postgres pgadmin

  

<span class="token comment"># Switch to WSL</span>

wsl

  

<span class="token comment"># Go to backend</span>

cd <span class="token operator">/</span>mnt<span class="token operator">/</span>c<span class="token operator">/</span>Users<span class="token operator">/</span><span class="token namespace">[YOUR-USERNAME]</span><span class="token operator">/</span>path<span class="token operator">/</span>to<span class="token operator">/</span>ngam<span class="token operator">-</span>je<span class="token operator">/</span>backend

  

<span class="token comment"># Start backend</span>

uv run uvicorn src<span class="token punctuation">.</span>app<span class="token punctuation">.</span>main:app <span class="token operator">--</span>reload <span class="token operator">--</span>host 0<span class="token punctuation">.</span>0<span class="token punctuation">.</span>0<span class="token punctuation">.</span>0 <span class="token operator">--</span>port 8000

</code></pre>
<h3 id="stop-everything">Stop everything:</h3>
<pre class=" language-bash"><code class="prism  language-bash">
<span class="token comment"># Stop backend: Press Ctrl+C in the terminal running uvicorn</span>

  

<span class="token comment"># Stop Docker (from project root in Windows PowerShell)</span>

docker-compose down

</code></pre>
<h3 id="view-docker-logs">View Docker logs:</h3>
<pre class=" language-bash"><code class="prism  language-bash">
docker-compose logs postgres

docker-compose logs pgadmin

</code></pre>
<h3 id="access-database-directly">Access database directly:</h3>
<pre class=" language-bash"><code class="prism  language-bash">
docker <span class="token function">exec</span> -it ngam-je-postgres psql -U ngamje -d ngamje_db

</code></pre>
<hr>
<h2 id="summary">Summary</h2>
<p>✅ Docker Desktop running</p>
<p>✅ WSL installed with Ubuntu</p>
<p>✅ .env file in backend folder with correct email</p>
<p>✅ PostgreSQL running on port 5432</p>
<p>✅ pgAdmin accessible at <a href="http://localhost:5050">http://localhost:5050</a></p>
<p>✅ Database tables created via Alembic migrations</p>
<p>✅ FastAPI backend running at <a href="http://localhost:8000">http://localhost:8000</a></p>
<p><strong>Key Insight:</strong> WSL bridges the gap between Windows and Linux, solving Docker networking issues and matching your teacher’s Linux environment! 🎉</p>

