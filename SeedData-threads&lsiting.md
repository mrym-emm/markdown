---


---

<h1 id="setup-instructions">Setup Instructions</h1>
<h2 id="backend-setup">2. Backend Setup</h2>
<p>Install dependencies:</p>
<pre class=" language-bash"><code class="prism  language-bash"><span class="token function">cd</span> backend
uv <span class="token function">sync</span>
</code></pre>
<p>Run database migrations:</p>
<pre class=" language-bash"><code class="prism  language-bash">uv run alembic upgrade <span class="token function">head</span>
</code></pre>
<p>Seed the database:<br>
Open pgAdmin, connect to your database, then:</p>
<ol>
<li>Right-click database → Query Tool</li>
<li>Open file: <code>backend/scripts/seed_data.sql</code></li>
<li>Click Execute (F5)</li>
</ol>
<p>You should see: “5 threads, 8 listings inserted”</p>
<p>Start backend:</p>
<pre class=" language-bash"><code class="prism  language-bash">uv run uvicorn src.app.main:app --reload --host 0.0.0.0 --port 8000
</code></pre>
<h2 id="frontend-setup">3. Frontend Setup</h2>
<p>Check environment variables:<br>
Make sure <code>frontend/.env.local</code> has:</p>
<pre><code>NEXT_PUBLIC_API_URL=http://localhost:8000
</code></pre>
<p>Start frontend:</p>
<pre class=" language-bash"><code class="prism  language-bash"><span class="token function">cd</span> frontend
<span class="token function">npm</span> run dev
</code></pre>
<h2 id="test">4. Test</h2>
<p>Visit <code>http://localhost:3000/threads</code> - you should see 5 threads from the database!</p>

