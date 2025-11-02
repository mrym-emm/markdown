---


---

<h1 id="how-to-verify-everything-is-working">How to Verify Everything is Working</h1>
<p>Run these checks in order:</p>
<h2 id="check-docker-containers-are-running">1. Check Docker containers are running:</h2>
<pre class=" language-powershell"><code class="prism  language-powershell"><span class="token comment"># In Windows PowerShell (not WSL)</span>
docker<span class="token operator">-</span>compose <span class="token function">ps</span>
</code></pre>
<p><strong>Expected output:</strong></p>
<pre><code>NAME               STATUS
ngam-je-pgadmin    Up X minutes
ngam-je-postgres   Up X minutes (healthy)
</code></pre>
<h2 id="check-pgadmin">2. Check pgAdmin:</h2>
<ol>
<li>Open browser: <a href="http://localhost:5050">http://localhost:5050</a></li>
<li>Should see pgAdmin dashboard</li>
<li>Expand <strong>Servers → Ngam-je Local → Databases → ngamje_db → Schemas → public → Tables</strong></li>
<li>Should see <code>users</code> and <code>alembic_version</code> tables</li>
</ol>
<h2 id="check-database-has-tables">3. Check database has tables:</h2>
<pre class=" language-bash"><code class="prism  language-bash"><span class="token comment"># In WSL</span>
docker <span class="token function">exec</span> -it ngam-je-postgres psql -U ngamje -d ngamje_db -c <span class="token string">"\dt"</span>
</code></pre>
<p><strong>Expected output:</strong></p>
<pre><code> Schema |      Name       | Type  | Owner
--------+-----------------+-------+--------
 public | alembic_version | table | ngamje
 public | users           | table | ngamje
(2 rows)
</code></pre>
<h2 id="check-backend-is-running">4. Check backend is running:</h2>
<p>In WSL, run:</p>
<pre class=" language-bash"><code class="prism  language-bash"><span class="token function">cd</span> /mnt/c/Users/<span class="token punctuation">[</span>YOUR-USERNAME<span class="token punctuation">]</span>/path/to/ngam-je/backend
uv run uvicorn src.app.main:app --reload --host 0.0.0.0 --port 8000
</code></pre>
<p><strong>Should see:</strong></p>
<pre><code>INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
INFO:     Application startup complete.
</code></pre>
<h2 id="test-api-endpoints-in-browser">5. Test API endpoints in browser:</h2>
<p>Open these URLs while backend is running:</p>
<ul>
<li><strong><a href="http://localhost:8000">http://localhost:8000</a></strong> - Should see: <code>{"message":"Welcome to Ngam-Je API"}</code></li>
<li><strong><a href="http://localhost:8000/docs">http://localhost:8000/docs</a></strong> - Should see Swagger UI (interactive API docs)</li>
<li><strong><a href="http://localhost:8000/api/v1/health">http://localhost:8000/api/v1/health</a></strong> - Should see: <code>{"status":"healthy"}</code></li>
</ul>
<h2 id="test-creating-a-user-optional">6. Test creating a user (Optional):</h2>
<p>In the browser at <a href="http://localhost:8000/docs:">http://localhost:8000/docs:</a></p>
<ol>
<li>Find <strong>POST /api/v1/auth/signup</strong></li>
<li>Click <strong>"Try it out"</strong></li>
<li>Fill in the request body:</li>
</ol>
<pre class=" language-json"><code class="prism  language-json">   <span class="token punctuation">{</span>
     <span class="token string">"email"</span><span class="token punctuation">:</span> <span class="token string">"test@example.com"</span><span class="token punctuation">,</span>
     <span class="token string">"username"</span><span class="token punctuation">:</span> <span class="token string">"testuser"</span><span class="token punctuation">,</span>
     <span class="token string">"password"</span><span class="token punctuation">:</span> <span class="token string">"testpassword123"</span>
   <span class="token punctuation">}</span>
</code></pre>
<ol start="4">
<li>Click <strong>"Execute"</strong></li>
<li>Should get <strong>201 Created</strong> response with user data</li>
</ol>
<p><strong>Then verify in pgAdmin:</strong></p>
<ul>
<li>Right-click <strong>Tables → users → View/Edit Data → All Rows</strong></li>
<li>Should see your test user!</li>
</ul>
<hr>
<h2 id="why-uv-run-uvicorn-src.app.mainapp---reload---host-0.0.0.0---port-8000">Why <code>uv run uvicorn src.app.main:app --reload --host 0.0.0.0 --port 8000</code>?</h2>
<p>Let me break down each part:</p>
<h3 id="uv-run"><code>uv run</code></h3>
<ul>
<li><strong>Why?</strong> Automatically uses the virtual environment (<code>.venv</code>) without manual activation</li>
<li><strong>Without it:</strong> You’d need to activate <code>.venv</code> first, which is annoying and error-prone</li>
<li><strong>Alternative:</strong> Manually run <code>.venv/bin/activate</code> then <code>uvicorn ...</code> (more steps)</li>
</ul>
<h3 id="uvicorn"><code>uvicorn</code></h3>
<ul>
<li><strong>What?</strong> ASGI server that runs FastAPI applications</li>
<li><strong>Why?</strong> FastAPI needs a server to handle HTTP requests</li>
<li><strong>Think of it as:</strong> The engine that runs your FastAPI app</li>
</ul>
<h3 id="src.app.mainapp"><code>src.app.main:app</code></h3>
<ul>
<li><code>src.app.main</code> - Python module path to your file (<code>src/app/main.py</code>)</li>
<li><code>:app</code> - Variable name inside <code>main.py</code> that holds your FastAPI application</li>
<li><strong>Why?</strong> Tells uvicorn exactly which app to run</li>
</ul>
<h3 id="reload"><code>--reload</code></h3>
<ul>
<li><strong>What?</strong> Auto-restarts server when you change code</li>
<li><strong>Why?</strong> So you don’t have to stop/start server every time you edit a file</li>
<li><strong>Only use in development!</strong> Not in production</li>
</ul>
<h3 id="host-0.0.0.0"><code>--host 0.0.0.0</code></h3>
<ul>
<li><strong>What?</strong> Listens on all network interfaces</li>
<li><strong>Why?</strong> Allows access from:
<ul>
<li><code>localhost</code> (your machine)</li>
<li><code>127.0.0.1</code> (loopback)</li>
<li>Your computer’s IP address (if needed)</li>
</ul>
</li>
<li><strong>Without it:</strong> Only accessible from localhost, which can cause issues</li>
</ul>
<h3 id="port-8000"><code>--port 8000</code></h3>
<ul>
<li><strong>What?</strong> Run server on port 8000</li>
<li><strong>Why?</strong> Standard port for backend APIs (doesn’t conflict with frontend on 3000)</li>
<li><strong>You can change it:</strong> <code>--port 3001</code> if port 8000 is in use</li>
</ul>
<hr>
<h2 id="alternative-commands-and-why-we-dont-use-them">Alternative Commands (and why we don’t use them):</h2>
<h3 id="❌-fastapi-dev-simpler-but-doesnt-work-in-your-setup">❌ <code>fastapi dev</code> (Simpler but doesn’t work in your setup)</h3>
<pre class=" language-bash"><code class="prism  language-bash">uv run fastapi dev
<span class="token comment"># Error: Need to install "fastapi[standard]"</span>
</code></pre>
<p><strong>Why it fails:</strong> Your project uses minimal FastAPI without the <code>[standard]</code> extras</p>
<h3 id="❌-manual-activation-more-steps">❌ Manual activation (More steps)</h3>
<pre class=" language-bash"><code class="prism  language-bash"><span class="token function">source</span> .venv/bin/activate  <span class="token comment"># Activate venv</span>
uvicorn src.app.main:app --reload --host 0.0.0.0 --port 8000
</code></pre>
<p><strong>Why we use <code>uv run</code> instead:</strong> One command vs two, less error-prone</p>
<h3 id="❌-without---reload-have-to-restart-manually">❌ Without <code>--reload</code> (Have to restart manually)</h3>
<pre class=" language-bash"><code class="prism  language-bash">uv run uvicorn src.app.main:app --host 0.0.0.0 --port 8000
</code></pre>
<p><strong>Why we add <code>--reload</code>:</strong> Auto-restart on code changes during development</p>
<hr>
<h2 id="quick-verification-checklist">Quick Verification Checklist</h2>
<p>✅ <code>docker-compose ps</code> shows both containers “Up”<br>
✅ <a href="http://localhost:5050">http://localhost:5050</a> shows pgAdmin<br>
✅ pgAdmin shows “users” table<br>
✅ <code>docker exec ... \dt</code> shows 2 tables<br>
✅ <code>uv run uvicorn ...</code> shows “Application startup complete”<br>
✅ <a href="http://localhost:8000/docs">http://localhost:8000/docs</a> shows Swagger UI<br>
✅ <a href="http://localhost:8000/api/v1/health">http://localhost:8000/api/v1/health</a> returns <code>{"status":"healthy"}</code></p>
<p><strong>If all ✅ = Everything is working! 🎉</strong></p>
<hr>
<h2 id="to-share-with-teammates">To Share with Teammates</h2>
<p>"Run these commands to verify setup works:</p>
<pre class=" language-bash"><code class="prism  language-bash"><span class="token comment"># 1. Check Docker</span>
docker-compose <span class="token function">ps</span>

<span class="token comment"># 2. Enter WSL and go to backend</span>
wsl
<span class="token function">cd</span> /mnt/c/Users/<span class="token punctuation">[</span>YOUR-USERNAME<span class="token punctuation">]</span>/path/to/ngam-je/backend

<span class="token comment"># 3. Start backend</span>
uv run uvicorn src.app.main:app --reload --host 0.0.0.0 --port 8000

<span class="token comment"># 4. In browser, test these URLs:</span>
<span class="token comment"># - http://localhost:8000/docs</span>
<span class="token comment"># - http://localhost:8000/api/v1/health</span>
</code></pre>
<p><strong>If all work = success!</strong>"</p>

