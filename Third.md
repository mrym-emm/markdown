---


---

<h1 id="complete-verification-steps-in-vscode---start-to-finish">Complete Verification Steps in VSCode - Start to Finish</h1>
<h2 id="step-1-open-project-in-vscode">Step 1: Open Project in VSCode</h2>
<ol>
<li>Open VSCode</li>
<li><strong>File → Open Folder</strong></li>
<li>Navigate to: <code>C:\Users\marya\OneDrive\Desktop\capstone_final\ngam-je</code></li>
<li>Click <strong>Select Folder</strong></li>
</ol>
<h2 id="step-2-open-terminal-in-vscode">Step 2: Open Terminal in VSCode</h2>
<p>Press <strong>Ctrl + `</strong> (backtick key, below Esc)</p>
<p>You’ll see a terminal panel at the bottom.</p>
<h2 id="step-3-check-docker-powershell">Step 3: Check Docker (PowerShell)</h2>
<p>Your terminal should already be in the project folder. Run:</p>
<pre class=" language-powershell"><code class="prism  language-powershell">docker<span class="token operator">-</span>compose <span class="token function">ps</span>
</code></pre>
<p><strong>Expected:</strong></p>
<pre><code>NAME               STATUS
ngam-je-pgadmin    Up X minutes
ngam-je-postgres   Up X minutes (healthy)
</code></pre>
<p><strong>If NOT running:</strong></p>
<pre class=" language-powershell"><code class="prism  language-powershell">docker<span class="token operator">-</span>compose up <span class="token operator">-</span>d postgres pgadmin
</code></pre>
<h2 id="step-4-switch-to-wsl-terminal">Step 4: Switch to WSL Terminal</h2>
<ol>
<li>In the terminal panel, click the <strong>dropdown arrow</strong> next to the <strong>+</strong> icon (top right of terminal panel).</li>
<li>Select <strong>“Ubuntu (WSL)”</strong> or <strong>"WSL"</strong></li>
</ol>
<p>Your terminal prompt changes to:</p>
<pre><code>marya@mm:/mnt/c/Users/marya/OneDrive/Desktop/capstone_final/ngam-je$
</code></pre>
<h2 id="step-5-navigate-to-backend">Step 5: Navigate to Backend</h2>
<pre class=" language-bash"><code class="prism  language-bash"><span class="token function">cd</span> backend
</code></pre>
<h2 id="step-6-add-uv-to-path">Step 6: Add uv to PATH</h2>
<pre class=" language-bash"><code class="prism  language-bash"><span class="token function">export</span> PATH<span class="token operator">=</span><span class="token string">"<span class="token variable">$HOME</span>/.local/bin:<span class="token variable">$PATH</span>"</span>
</code></pre>
<p>Verify:</p>
<pre class=" language-bash"><code class="prism  language-bash">uv --version
</code></pre>
<h2 id="step-7-check-database-tables">Step 7: Check Database Tables</h2>
<pre class=" language-bash"><code class="prism  language-bash">docker <span class="token function">exec</span> -it ngam-je-postgres psql -U ngamje -d ngamje_db -c <span class="token string">"\dt"</span>
</code></pre>
<p><strong>Expected:</strong></p>
<pre><code> public | alembic_version | table | ngamje
 public | users           | table | ngamje
</code></pre>
<h2 id="step-8-start-backend">Step 8: Start Backend</h2>
<pre class=" language-bash"><code class="prism  language-bash">uv run uvicorn src.app.main:app --reload --host 0.0.0.0 --port 8000
</code></pre>
<p><strong>Expected:</strong></p>
<pre><code>INFO:     Application startup complete.
</code></pre>
<h2 id="step-9-test-api-without-leaving-vscode">Step 9: Test API (Without Leaving VSCode!)</h2>
<h3 id="option-1-use-built-in-browser-preview">Option 1: Use built-in browser preview</h3>
<ol>
<li>Press <strong>Ctrl + Shift + P</strong></li>
<li>Type “Simple Browser”</li>
<li>Enter URL: <code>http://localhost:8000/docs</code></li>
</ol>
<h3 id="option-2-use-your-regular-browser">Option 2: Use your regular browser</h3>
<ul>
<li><a href="http://localhost:8000/docs">http://localhost:8000/docs</a></li>
</ul>
<h2 id="step-10-stop-backend">Step 10: Stop Backend</h2>
<p>In the terminal, press <strong>Ctrl + C</strong></p>
<hr>
<h2 id="pro-tips-for-vscode">Pro Tips for VSCode</h2>
<h3 id="multiple-terminals">Multiple Terminals</h3>
<p>Click the <strong>+</strong> icon to open multiple terminals:</p>
<ul>
<li><strong>Terminal 1 (PowerShell):</strong> For docker commands</li>
<li><strong>Terminal 2 (WSL):</strong> For running backend</li>
<li><strong>Terminal 3 (WSL):</strong> For other commands</li>
</ul>
<h3 id="split-terminal">Split Terminal</h3>
<p>Click the <strong>split icon</strong> (next to +) to have terminals side-by-side!</p>
<h3 id="default-to-wsl">Default to WSL</h3>
<p>Make WSL your default terminal:</p>
<ol>
<li>Press <strong>Ctrl + Shift + P</strong></li>
<li>Type “Terminal: Select Default Profile”</li>
<li>Choose <strong>"Ubuntu (WSL)"</strong></li>
</ol>
<p>Now every new terminal opens in WSL automatically!</p>
<hr>
<h2 id="complete-vscode-workflow">Complete VSCode Workflow</h2>
<pre class=" language-bash"><code class="prism  language-bash"><span class="token comment"># Open VSCode → Open ngam-je folder</span>
<span class="token comment"># Press Ctrl + ` to open terminal</span>
<span class="token comment"># Switch to WSL terminal (dropdown → Ubuntu)</span>

<span class="token function">cd</span> backend
<span class="token function">export</span> PATH<span class="token operator">=</span><span class="token string">"<span class="token variable">$HOME</span>/.local/bin:<span class="token variable">$PATH</span>"</span>
uv run uvicorn src.app.main:app --reload --host 0.0.0.0 --port 8000

<span class="token comment"># Click the URL in terminal (Ctrl + Click)</span>
<span class="token comment"># Or open http://localhost:8000/docs in browser</span>

<span class="token comment"># When done: Ctrl + C to stop</span>
</code></pre>
<p><strong>Everything in one window! Code, terminal, and browser preview. 🚀</strong></p>
<hr>
<h2 id="quick-verification-checklist-vscode-edition">Quick Verification Checklist (VSCode Edition)</h2>
<p>✅ Opened ngam-je folder in VSCode<br>
✅ Pressed Ctrl + <code>to open terminal ✅ Switched to WSL terminal ✅ Ran</code>docker-compose ps<code>(shows Up) ✅</code>cd backend<code>✅</code>export PATH="<span class="katex--inline"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mi>H</mi><mi>O</mi><mi>M</mi><mi>E</mi><mi mathvariant="normal">/</mi><mi mathvariant="normal">.</mi><mi>l</mi><mi>o</mi><mi>c</mi><mi>a</mi><mi>l</mi><mi mathvariant="normal">/</mi><mi>b</mi><mi>i</mi><mi>n</mi><mo>:</mo></mrow><annotation encoding="application/x-tex">HOME/.local/bin:</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height: 1em; vertical-align: -0.25em;"></span><span class="mord mathnormal" style="margin-right: 0.08125em;">H</span><span class="mord mathnormal" style="margin-right: 0.05764em;">OME</span><span class="mord">/.</span><span class="mord mathnormal" style="margin-right: 0.01968em;">l</span><span class="mord mathnormal">oc</span><span class="mord mathnormal">a</span><span class="mord mathnormal" style="margin-right: 0.01968em;">l</span><span class="mord">/</span><span class="mord mathnormal">bin</span><span class="mspace" style="margin-right: 0.277778em;"></span><span class="mrel">:</span></span></span></span></span>PATH"<code>✅</code>uv --version<code>works ✅</code>docker exec … \dt<code>shows tables ✅</code>uv run uvicorn …` shows “startup complete”<br>
✅ Opened <a href="http://localhost:8000/docs">http://localhost:8000/docs</a> in browser<br>
✅ Tested health endpoint<br>
✅ Ctrl + C to stop backend</p>
<p><strong>All in VSCode = Super convenient! 🎉</strong></p>

