```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />

  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  />

  <meta
    name="description"
    content="Prompt to Animated Code Generator"
  />

  <title>Prompt → Animated Code</title>

  <style>
    /* =========================================================
       RESET
    ========================================================= */

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      min-width: 320px;
      min-height: 100vh;

      font-family:
        Inter,
        ui-sans-serif,
        system-ui,
        -apple-system,
        BlinkMacSystemFont,
        "Segoe UI",
        sans-serif;

      background: #05070b;
      color: #f5f7fa;

      overflow-x: hidden;
    }

    button,
    textarea {
      font: inherit;
    }

    button {
      cursor: pointer;
    }

    button:disabled {
      cursor: not-allowed;
      opacity: 0.5;
    }


    /* =========================================================
       APP
    ========================================================= */

    .app {
      position: relative;

      min-height: 100vh;

      overflow: hidden;

      background:
        radial-gradient(
          circle at 10% 10%,
          rgba(0, 220, 255, 0.13),
          transparent 30%
        ),
        radial-gradient(
          circle at 90% 90%,
          rgba(120, 70, 255, 0.13),
          transparent 30%
        ),
        #05070b;

      transition:
        background 300ms ease,
        color 300ms ease;
    }

    .app.light {
      color: #111827;

      background:
        radial-gradient(
          circle at 10% 10%,
          rgba(0, 170, 220, 0.12),
          transparent 30%
        ),
        #f4f7fb;
    }


    /* =========================================================
       BACKGROUND GRID
    ========================================================= */

    .grid-background {
      position: fixed;
      inset: 0;

      pointer-events: none;

      opacity: 0.18;

      background-image:
        linear-gradient(
          rgba(255,255,255,0.035) 1px,
          transparent 1px
        ),
        linear-gradient(
          90deg,
          rgba(255,255,255,0.035) 1px,
          transparent 1px
        );

      background-size: 50px 50px;

      mask-image:
        linear-gradient(
          to bottom,
          black,
          transparent
        );
    }


    /* =========================================================
       FLOATING AI NODES
    ========================================================= */

    .animation-layer {
      position: fixed;
      inset: 0;

      pointer-events: none;

      overflow: hidden;

      z-index: 0;
    }

    .node {
      position: absolute;

      width: 7px;
      height: 7px;

      border-radius: 50%;

      background: #00d9ff;

      box-shadow:
        0 0 15px #00d9ff,
        0 0 35px rgba(0, 217, 255, 0.5);

      opacity: 0.5;

      animation:
        floatNode 8s infinite ease-in-out,
        pulseNode 3s infinite ease-in-out;
    }

    .node:nth-child(1) {
      left: 8%;
      top: 20%;
    }

    .node:nth-child(2) {
      left: 23%;
      top: 75%;

      animation-delay: 1s;
    }

    .node:nth-child(3) {
      left: 48%;
      top: 18%;

      animation-delay: 2s;
    }

    .node:nth-child(4) {
      left: 72%;
      top: 40%;

      animation-delay: 3s;
    }

    .node:nth-child(5) {
      left: 88%;
      top: 75%;

      animation-delay: 4s;
    }

    .node:nth-child(6) {
      left: 55%;
      top: 85%;

      animation-delay: 5s;
    }

    @keyframes floatNode {
      0%,
      100% {
        transform: translate3d(0, 0, 0);
      }

      50% {
        transform: translate3d(
          0,
          -30px,
          0
        );
      }
    }

    @keyframes pulseNode {
      0%,
      100% {
        opacity: 0.25;
        transform: scale(0.8);
      }

      50% {
        opacity: 0.8;
        transform: scale(1.4);
      }
    }


    /* =========================================================
       MAIN CONTAINER
    ========================================================= */

    .container {
      position: relative;

      z-index: 2;

      width: min(
        1200px,
        calc(100% - 32px)
      );

      margin: auto;

      padding:
        55px
        0
        35px;
    }


    /* =========================================================
       HEADER
    ========================================================= */

    header {
      display: flex;

      align-items: flex-start;

      justify-content: space-between;

      gap: 30px;

      margin-bottom: 40px;
    }

    .eyebrow {
      margin-bottom: 14px;

      font-size: 12px;

      font-weight: 800;

      letter-spacing: 0.2em;

      color: #00d9ff;
    }

    h1 {
      font-size:
        clamp(
          42px,
          7vw,
          82px
        );

      line-height: 0.95;

      letter-spacing: -0.06em;
    }

    h1 span {
      color: #00d9ff;

      opacity: 0.8;
    }

    .subtitle {
      max-width: 650px;

      margin-top: 25px;

      font-size: 17px;

      line-height: 1.7;

      opacity: 0.6;
    }


    /* =========================================================
       CONTROLS
    ========================================================= */

    .controls {
      display: flex;

      gap: 10px;

      flex-shrink: 0;
    }

    .button {
      border: 1px solid
        rgba(255,255,255,0.12);

      border-radius: 12px;

      padding:
        12px
        18px;

      color: inherit;

      background:
        rgba(255,255,255,0.06);

      transition:
        transform 180ms ease,
        background 180ms ease;
    }

    .button:hover:not(:disabled) {
      transform: translateY(-2px);

      background:
        rgba(255,255,255,0.1);
    }

    .primary {
      border: none;

      color: white;

      background:
        linear-gradient(
          135deg,
          #0077ff,
          #00c6ff
        );

      box-shadow:
        0 10px 30px
        rgba(0,150,255,0.2);
    }


    /* =========================================================
       STATUS
    ========================================================= */

    .status {
      margin-bottom: 20px;

      padding: 13px 16px;

      border:
        1px solid
        rgba(0,200,255,0.15);

      border-radius: 12px;

      color: #bcefff;

      background:
        rgba(0,180,255,0.07);

      animation:
        statusIn 250ms ease;
    }

    @keyframes statusIn {
      from {
        opacity: 0;
        transform: translateY(-5px);
      }

      to {
        opacity: 1;
        transform: translateY(0);
      }
    }


    /* =========================================================
       WORKSPACE
    ========================================================= */

    .workspace {
      display: grid;

      grid-template-columns:
        minmax(300px, 0.8fr)
        minmax(400px, 1.2fr);

      gap: 18px;
    }

    .card {
      border:
        1px solid
        rgba(255,255,255,0.12);

      border-radius: 22px;

      background:
        rgba(255,255,255,0.045);

      backdrop-filter: blur(18px);

      box-shadow:
        0 25px 80px
        rgba(0,0,0,0.25);

      transition:
        border-color 250ms ease,
        transform 250ms ease;
    }

    .card:hover {
      border-color:
        rgba(0,210,255,0.35);
    }


    /* =========================================================
       PROMPT PANEL
    ========================================================= */

    .prompt-panel {
      padding: 25px;
    }

    .panel-title {
      margin-bottom: 12px;

      font-size: 17px;

      font-weight: 750;
    }

    textarea {
      width: 100%;

      min-height: 220px;

      resize: vertical;

      padding: 17px;

      border:
        1px solid
        rgba(255,255,255,0.13);

      border-radius: 15px;

      outline: none;

      color: inherit;

      background:
        rgba(0,0,0,0.2);

      line-height: 1.6;

      transition:
        border-color 180ms ease,
        box-shadow 180ms ease;
    }

    textarea::placeholder {
      color: currentColor;

      opacity: 0.35;
    }

    textarea:focus {
      border-color: #00bfff;

      box-shadow:
        0 0 0 4px
        rgba(0,191,255,0.1);
    }

    .help {
      margin:
        10px
        0
        20px;

      font-size: 13px;

      opacity: 0.45;
    }

    .convert-button {
      width: 100%;

      padding: 14px;

      border: none;

      border-radius: 13px;

      font-weight: 750;

      color: white;

      background:
        linear-gradient(
          135deg,
          #006eff,
          #00c6ff
        );

      transition:
        transform 180ms ease,
        box-shadow 180ms ease;
    }

    .convert-button:hover:not(:disabled) {
      transform: translateY(-2px);

      box-shadow:
        0 12px 35px
        rgba(0,150,255,0.25);
    }


    /* =========================================================
       CODE PANEL
    ========================================================= */

    .code-panel {
      overflow: hidden;
    }

    .code-header {
      display: grid;

      grid-template-columns:
        1fr
        auto
        1fr;

      align-items: center;

      min-height: 55px;

      padding:
        0
        18px;

      border-bottom:
        1px solid
        rgba(255,255,255,0.1);

      font-size: 13px;
    }

    .dots {
      display: flex;

      gap: 6px;
    }

    .dot {
      width: 8px;
      height: 8px;

      border-radius: 50%;

      background: currentColor;

      opacity: 0.35;
    }

    .language {
      justify-self: end;

      opacity: 0.45;
    }

    .code-container {
      min-height: 480px;

      max-height: 650px;

      overflow: auto;

      padding: 25px;

      background:
        rgba(0,0,0,0.18);
    }

    pre {
      margin: 0;

      white-space: pre;

      font-family:
        "SFMono-Regular",
        Consolas,
        "Liberation Mono",
        monospace;

      font-size: 14px;

      line-height: 1.8;
    }

    .empty {
      min-height: 420px;

      display: grid;

      place-items: center;

      text-align: center;

      opacity: 0.35;
    }


    /* =========================================================
       CODE LINE ANIMATION
    ========================================================= */

    .code-line {
      display: block;

      opacity: 0;

      transform:
        translateX(-8px);

      animation:
        revealLine
        250ms
        ease
        forwards;
    }

    @keyframes revealLine {
      to {
        opacity: 1;

        transform:
          translateX(0);
      }
    }


    /* =========================================================
       LOADING
    ========================================================= */

    .loading {
      min-height: 420px;

      display: flex;

      align-items: center;

      justify-content: center;

      gap: 12px;

      opacity: 0.6;
    }

    .spinner {
      width: 18px;
      height: 18px;

      border:
        2px solid
        currentColor;

      border-right-color:
        transparent;

      border-radius: 50%;

      animation:
        spin
        700ms
        linear
        infinite;
    }

    @keyframes spin {
      to {
        transform: rotate(360deg);
      }
    }


    /* =========================================================
       FOOTER
    ========================================================= */

    footer {
      display: flex;

      justify-content: center;

      gap: 20px;

      margin-top: 30px;

      font-size: 12px;

      opacity: 0.4;
    }


    /* =========================================================
       RESPONSIVE
    ========================================================= */

    @media (max-width: 850px) {

      .container {
        padding-top: 35px;
      }

      header {
        flex-direction: column;
      }

      .controls {
        width: 100%;
      }

      .controls .button {
        flex: 1;
      }

      .workspace {
        grid-template-columns: 1fr;
      }

    }

    @media (max-width: 500px) {

      .container {
        width:
          calc(100% - 20px);
      }

      h1 {
        font-size: 48px;
      }

      .workspace {
        gap: 12px;
      }

      .prompt-panel {
        padding: 18px;
      }

      .code-container {
        padding: 17px;
      }

      footer {
        flex-wrap: wrap;
      }

    }


    /* =========================================================
       ACCESSIBILITY
    ========================================================= */

    @media (prefers-reduced-motion: reduce) {

      *,
      *::before,
      *::after {
        animation-duration:
          0.01ms !important;

        animation-iteration-count:
          1 !important;

        transition-duration:
          0.01ms !important;

        scroll-behavior:
          auto !important;
      }

    }
  </style>
</head>


<body>

  <div class="app" id="app">

    <!-- Animated background -->

    <div
      class="grid-background"
      aria-hidden="true"
    ></div>

    <div
      class="animation-layer"
      aria-hidden="true"
    >
      <span class="node"></span>
      <span class="node"></span>
      <span class="node"></span>
      <span class="node"></span>
      <span class="node"></span>
      <span class="node"></span>
    </div>


    <main class="container">

      <!-- HEADER -->

      <header>

        <div>

          <div class="eyebrow">
            AI CODE GENERATOR
          </div>

          <h1>
            Prompt
            <span>→</span>
            <br />
            Animated Code
          </h1>

          <p class="subtitle">
            Describe what you want to build and
            watch representative code appear
            line by line with a smooth typing effect.
          </p>

        </div>


        <div class="controls">

          <button
            class="button"
            id="themeButton"
            type="button"
          >
            ☀ Light
          </button>

          <button
            class="button"
            id="copyButton"
            type="button"
            disabled
          >
            Copy Code
          </button>

        </div>

      </header>


      <!-- STATUS -->

      <div
        class="status"
        id="status"
        role="status"
        aria-live="polite"
        hidden
      ></div>


      <!-- WORKSPACE -->

      <section class="workspace">


        <!-- PROMPT -->

        <form
          class="card prompt-panel"
          id="promptForm"
        >

          <div class="panel-title">
            Describe the code you want
          </div>

          <label
            for="prompt"
            class="sr-only"
          >
            Enter a code generation prompt
          </label>

          <textarea
            id="prompt"
            name="prompt"
            placeholder="Example:

Create a React button

Create a Python hello world program

Create an API fetch function

Create a Todo app"
            aria-describedby="prompt-help"
          ></textarea>

          <p
            class="help"
            id="prompt-help"
          >
            Describe the component,
            application, function, or workflow
            you want to generate.
          </p>

          <button
            class="convert-button"
            id="convertButton"
            type="submit"
          >
            Convert →
          </button>

        </form>


        <!-- CODE OUTPUT -->

        <section
          class="card code-panel"
          aria-labelledby="outputTitle"
        >

          <div class="code-header">

            <div class="dots">
              <span class="dot"></span>
              <span class="dot"></span>
              <span class="dot"></span>
            </div>

            <span id="outputTitle">
              Code Output
            </span>

            <span
              class="language"
              id="language"
            >
              —
            </span>

          </div>


          <div
            class="code-container"
            id="codeContainer"
            aria-live="polite"
            aria-busy="false"
          >

            <div class="empty">
              Your generated code
              will appear here.
            </div>

          </div>

        </section>

      </section>


      <footer>
        <span>HTML</span>
        <span>CSS</span>
        <span>JavaScript</span>
        <span>CSS Animations</span>
      </footer>

    </main>

  </div>


  <script>

    /* =========================================================
       ELEMENTS
    ========================================================= */

    const app =
      document.getElementById("app");

    const form =
      document.getElementById("promptForm");

    const promptInput =
      document.getElementById("prompt");

    const convertButton =
      document.getElementById("convertButton");

    const codeContainer =
      document.getElementById("codeContainer");

    const language =
      document.getElementById("language");

    const outputTitle =
      document.getElementById("outputTitle");

    const copyButton =
      document.getElementById("copyButton");

    const themeButton =
      document.getElementById("themeButton");

    const status =
      document.getElementById("status");


    /* =========================================================
       CODE TEMPLATES
    ========================================================= */

    const templates = [

      {
        keywords: ["react", "button"],

        language: "tsx",

        title: "Animated React Button",

        code:
`import React from "react";

export function AnimatedButton() {
  return (
    <button className="animated-button">
      Click me
    </button>
  );
}`
      },


      {
        keywords: ["api", "fetch"],

        language: "typescript",

        title: "API Fetch Function",

        code:
`async function fetchUsers() {
  const response = await fetch(
    "https://api.example.com/users"
  );

  if (!response.ok) {
    throw new Error("Request failed");
  }

  return response.json();
}`
      },


      {
        keywords: ["python", "hello"],

        language: "python",

        title: "Python Hello World",

        code:
`def main():
    message = "Hello, World!"
    print(message)


if __name__ == "__main__":
    main()`
      },


      {
        keywords: ["todo", "app"],

        language: "tsx",

        title: "Todo App",

        code:
`import { useState } from "react";

export default function TodoApp() {
  const [todos, setTodos] = useState<string[]>([]);

  function addTodo(todo: string) {
    setTodos((items) => [...items, todo]);
  }

  return (
    <div>
      <h1>Todo App</h1>
    </div>
  );
}`
      },


      {
        keywords: ["flask", "api"],

        language: "python",

        title: "Flask REST API",

        code:
`from flask import Flask, jsonify

app = Flask(__name__)


@app.get("/api/health")
def health():
    return jsonify({
        "status": "ok"
    })


if __name__ == "__main__":
    app.run(debug=True)`
      },


      {
        keywords: ["sql", "users"],

        language: "sql",

        title: "SQL User Query",

        code:
`SELECT
    id,
    name,
    email
FROM users
WHERE active = TRUE
ORDER BY name ASC;`
      }

    ];


    /* =========================================================
       FALLBACK
    ========================================================= */

    const fallback = {

      language: "typescript",

      title: "Generated TypeScript",

      code:
`function generatedSolution() {
  const prompt = "user request";

  console.log(
    "Implementing:",
    prompt
  );
}

generatedSolution();`

    };


    /* =========================================================
       GENERATOR
    ========================================================= */

    function generateCode(prompt) {

      const normalized =
        prompt
          .toLowerCase()
          .trim();


      const match =
        templates.find(template =>

          template.keywords.every(keyword =>
            normalized.includes(keyword)
          )

        );


      return match || fallback;

    }


    /* =========================================================
       STATUS
    ========================================================= */

    function showStatus(message) {

      status.textContent =
        message;

      status.hidden = false;

    }


    function hideStatus() {

      status.hidden = true;

      status.textContent = "";

    }


    /* =========================================================
       CODE ANIMATION
    ========================================================= */

    async function animateCode(result) {

      codeContainer.innerHTML = "";

      codeContainer.setAttribute(
        "aria-busy",
        "true"
      );

      outputTitle.textContent =
        result.title;

      language.textContent =
        result.language;


      const lines =
        result.code.split("\n");


      const pre =
        document.createElement("pre");


      const code =
        document.createElement("code");


      pre.appendChild(code);

      codeContainer.appendChild(pre);


      for (
        let i = 0;
        i < lines.length;
        i++
      ) {

        const line =
          document.createElement("span");


        line.className =
          "code-line";


        line.style.animationDelay =
          `${i * 25}ms`;


        line.textContent =
          lines[i] || "\u00A0";


        code.appendChild(line);


        await delay(80);

      }


      codeContainer.setAttribute(
        "aria-busy",
        "false"
      );

    }


    function delay(ms) {

      return new Promise(resolve =>
        setTimeout(resolve, ms)
      );

    }


    /* =========================================================
       CONVERT
    ========================================================= */

    form.addEventListener(
      "submit",
      async event => {

        event.preventDefault();


        const prompt =
          promptInput.value.trim();


        if (!prompt) {

          showStatus(
            "Please enter a prompt first."
          );

          promptInput.focus();

          return;

        }


        hideStatus();


        convertButton.disabled =
          true;


        copyButton.disabled =
          true;


        convertButton.textContent =
          "Generating...";


        codeContainer.innerHTML = `

          <div class="loading">

            <span class="spinner"></span>

            Generating code...

          </div>

        `;


        await delay(700);


        try {

          const result =
            generateCode(prompt);


          await animateCode(result);


          copyButton.disabled =
            false;


          showStatus(
            "Code generated successfully."
          );


        } catch (error) {

          showStatus(
            "Something went wrong while generating the code."
          );

        } finally {

          convertButton.disabled =
            false;

          convertButton.textContent =
            "Convert →";

        }

      }
    );


    /* =========================================================
       COPY
    ========================================================= */

    copyButton.addEventListener(
      "click",
      async () => {

        const code =
          codeContainer
            .querySelector("code");


        if (!code) return;


        try {

          await navigator.clipboard.writeText(
            code.textContent
          );


          showStatus(
            "Code copied to clipboard."
          );


          copyButton.textContent =
            "Copied ✓";


          setTimeout(() => {

            copyButton.textContent =
              "Copy Code";

          }, 1500);


        } catch {

          showStatus(
            "Unable to copy the code."
          );

        }

      }
    );


    /* =========================================================
       THEME
    ========================================================= */

    let darkMode = true;


    themeButton.addEventListener(
      "click",
      () => {

        darkMode =
          !darkMode;


        if (darkMode) {

          app.classList.remove(
            "light"
          );

          themeButton.textContent =
            "☀ Light";

        } else {

          app.classList.add(
            "light"
          );

          themeButton.textContent =
            "☾ Dark";

        }

      }
    );


    /* =========================================================
       KEYBOARD SHORTCUT
    ========================================================= */

    document.addEventListener(
      "keydown",
      event => {

        if (
          (event.ctrlKey ||
           event.metaKey) &&
          event.key === "Enter"
        ) {

          form.requestSubmit();

        }

      }
    );

  </script>

</body>
</html>
```
