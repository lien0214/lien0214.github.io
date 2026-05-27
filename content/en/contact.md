---
title: "Contact"
url: /en/contact/
summary: contact
description: "Contact Yi-Wei Lien — Backend & AI Engineer at NTU. Reach out for research collaboration, job conversations, or engineering discussions."
ShowToc: false
ShowReadingTime: false
hidemeta: true
---

<div class="contact-intro">
  Feel free to reach out for research discussions, collaboration, or just to say hi.
</div>

<div class="contact-list">

  <a href="mailto:ted20030214@gmail.com" class="contact-item">
    <span class="contact-item-icon">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/></svg>
    </span>
    <div>
      <div class="contact-item-label">Email</div>
      <div class="contact-item-value">ted20030214@gmail.com</div>
    </div>
  </a>

  <a href="https://www.linkedin.com/in/yi-wei-lien/" target="_blank" rel="noopener" class="contact-item">
    <span class="contact-item-icon">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"/><rect x="2" y="9" width="4" height="12"/><circle cx="4" cy="4" r="2"/></svg>
    </span>
    <div>
      <div class="contact-item-label">LinkedIn</div>
      <div class="contact-item-value">linkedin.com/in/yi-wei-lien</div>
    </div>
  </a>

  <a href="https://github.com/lien0214" target="_blank" rel="noopener" class="contact-item">
    <span class="contact-item-icon">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z"/></svg>
    </span>
    <div>
      <div class="contact-item-label">GitHub</div>
      <div class="contact-item-value">github.com/lien0214</div>
    </div>
  </a>

  <a href="tel:+886902323591" class="contact-item">
    <span class="contact-item-icon">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07A19.5 19.5 0 0 1 4.5 12.5a19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 3.41 2h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L7.91 9.91a16 16 0 0 0 6 6l.77-.77a2 2 0 0 1 2.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0 1 22 17z"/></svg>
    </span>
    <div>
      <div class="contact-item-label">Phone</div>
      <div class="contact-item-value">(+886) 902-323-591</div>
    </div>
  </a>

</div>

---

<div class="cf-section">
  <h3 class="cf-heading">Send a message</h3>

  <!-- Google Sign-In (auto-fills name + email if used) -->
  <div id="cf-gsi-wrap">
    <div id="g_id_onload"
         data-client_id="{{ site.Params.googleClientID }}"
         data-callback="cfGoogleSignIn"
         data-auto_select="false"
         data-itp_support="true">
    </div>
    <div class="g_id_signin"
         data-type="standard"
         data-size="medium"
         data-theme="outline"
         data-text="signin_with"
         data-shape="pill"
         data-logo_alignment="left">
    </div>
    <p class="cf-gsi-hint">Sign in to auto-fill your name and email, or fill them in manually below.</p>
  </div>

  <form id="cf-form" class="cf-form" novalidate>
    <div class="cf-row">
      <div class="cf-field">
        <label for="cf-name">Name</label>
        <input id="cf-name" name="name" type="text" placeholder="Your name" autocomplete="name" required>
      </div>
      <div class="cf-field">
        <label for="cf-email">Email</label>
        <input id="cf-email" name="email" type="email" placeholder="you@example.com" autocomplete="email" required>
      </div>
    </div>
    <div class="cf-field">
      <label for="cf-message">Message</label>
      <textarea id="cf-message" name="message" rows="5" placeholder="What's on your mind?" required></textarea>
    </div>
    <button type="submit" id="cf-submit" class="cf-btn">
      <span id="cf-btn-label">Send</span>
      <span id="cf-btn-spinner" class="cf-spinner" style="display:none">⏳</span>
    </button>
  </form>

  <div id="cf-success" class="cf-notice cf-notice-ok" style="display:none">
    ✓ Message sent — I'll get back to you soon.
  </div>
  <div id="cf-error" class="cf-notice cf-notice-err" style="display:none">
    Something went wrong. Email me directly at <a href="mailto:ted20030214@gmail.com">ted20030214@gmail.com</a>.
  </div>
</div>

<script>
(function () {
  var SCRIPT_URL = '{{ site.Params.contactScriptURL }}';
  var CLIENT_ID  = '{{ site.Params.googleClientID }}';

  /* ── Google Sign-In callback ── */
  window.cfGoogleSignIn = function (response) {
    try {
      var payload = JSON.parse(atob(response.credential.split('.')[1].replace(/-/g,'+').replace(/_/g,'/')));
      var nameEl  = document.getElementById('cf-name');
      var emailEl = document.getElementById('cf-email');
      if (nameEl  && !nameEl.value)  nameEl.value  = payload.name  || '';
      if (emailEl && !emailEl.value) emailEl.value = payload.email || '';
      var wrap = document.getElementById('cf-gsi-wrap');
      if (wrap) wrap.style.display = 'none';
    } catch(e) {}
  };

  /* ── Hide GSI block if no client ID configured ── */
  if (!CLIENT_ID) {
    var wrap = document.getElementById('cf-gsi-wrap');
    if (wrap) wrap.style.display = 'none';
  } else {
    /* Load GSI script dynamically */
    var s = document.createElement('script');
    s.src = 'https://accounts.google.com/gsi/client';
    s.async = true; s.defer = true;
    document.head.appendChild(s);
  }

  /* ── Form submission ── */
  var form    = document.getElementById('cf-form');
  var success = document.getElementById('cf-success');
  var error   = document.getElementById('cf-error');
  var btnLabel  = document.getElementById('cf-btn-label');
  var btnSpinner = document.getElementById('cf-btn-spinner');

  if (!form) return;

  form.addEventListener('submit', function (e) {
    e.preventDefault();
    if (!SCRIPT_URL) { error.style.display = 'block'; return; }

    var name    = document.getElementById('cf-name').value.trim();
    var email   = document.getElementById('cf-email').value.trim();
    var message = document.getElementById('cf-message').value.trim();
    if (!name || !email || !message) return;

    btnLabel.style.display  = 'none';
    btnSpinner.style.display = 'inline';
    document.getElementById('cf-submit').disabled = true;

    fetch(SCRIPT_URL, {
      method: 'POST',
      mode: 'no-cors',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ name: name, email: email, message: message })
    }).then(function () {
      form.style.display    = 'none';
      success.style.display = 'block';
    }).catch(function () {
      btnLabel.style.display   = 'inline';
      btnSpinner.style.display = 'none';
      document.getElementById('cf-submit').disabled = false;
      error.style.display = 'block';
    });
  });
})();
</script>
