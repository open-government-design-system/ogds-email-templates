This repo re-creates the [USWDS styles](https://designsystem.digital.gov/) in a way that works with emails.

## Why?

Email clients don't render content in the same way that web browsers do. There is a limited set of CSS styles that will render in each email client. An email that looks great in GMail might look very strange in Yahoo Mail or Outlook (or vice versa!)

When developing code to use in email templates, you should make sure it works across email clients.

## Screenshots of Email Examples

### Vanilla HTML

<details>
<summary>
Vanilla HTML email on desktop
</summary>

![Vanilla HTML email on desktop](./screenshots/Vanilla%20HTML%20-%20Desktop.png)

</details>

<details>
<summary>
Vanilla HTML email on mobile
</summary>

![Vanilla HTML email on mobile](./screenshots/Vanilla%20HTML%20-%20Mobile.png)

</details>

### MJML

<details>
<summary>
MJML email on desktop
</summary>

![MJML email on desktop](./screenshots/MJML%20-%20Desktop.png)

</details>

<details>
<summary>
MJML email on mobile
</summary>

![MJML email on mobile](./screenshots/MJML%20-%20Mobile.png)

</details>

## How to author effective email templates

Three tips for writing effective emails:

1. Start with a **standard base template** that is designed with accessibility and cross-browser support in mind (this repo).
2. When developing additional styles, make sure to consider **which styles are widely-supported across email clients**, such as referencing the [CanIEmail](https://caniemail.com) project.
3. **Test your emails** across email clients, using a tool such as Email on Acid.

## This project: Two Layers

There are two layers:

1. Basic css that works across email clients (we use this approach as much as possible)
2. For some elements, like buttons, it is difficult to get those to work without additional support. There are tools for this such as [MJML](https://mjml.io/).

### Typography can work with CSS alone

Most of the CSS for USWDS-styled emails can be achieved by including some CSS. This "base CSS" is the foundation of the OGDS Email Templates.

Features:

- The base CSS includes typography defaults for text, headings, and lists, that match the [USWDS Prose element](https://designsystem.digital.gov/components/prose/).
- We recommend leaving the font family as the default one for the email client (`initial`). Two reasons:
  - The default font respects font sizing settings on iOS; custom fonts do not
  - Plain text emails generally perform better than overly-styled emails

Two reference files:

1. The styles are in the `styles.css` file.
2. An example email is in the `template_vanilla_html.html` file.

Note: this vanilla html example does not have a reliable layout, or buttons.

### Layout and Buttons require additional tooling (MJML, etc)

Some things can NOT be done with CSS alone in emails. The biggest two are **layout** and **buttons**. These depend on the HTML nodes being structured in a certain way. For example, the `<button>` element is widely unsupported in email clients. There are several workarounds to get buttons to work, such as "tables in tables" approach, and the "not-exactly-SVG" approach.

If you need these fetures, you will need some email tooling. Many platforms come with their own email tooling (Mailchimp, GovDelivery, Salesforce). These platforms usually do provide layout and button support.

These are the changes you'll need to do in that tool to match the example templaet in this repo:

- Default text styling
  - Only if needed (many tools specify this, and it would override the css if they do – but we can put them “back” to this configuration)
  - color="#171717"
  - line-height="1.5"
  - font-size="16px" (leave blank if possible to leave it to CSS, or put this in if you must override your tool's setting)
- Email body element (parent div of everything)
  - background-color="#f0f0f0"
  - width="560px" this is close to the width of USWDS Prose, an approximation
- Each sub-section
  - padding-left="16px" padding-right="16px"
  - padding-top="0px" padding-bottom="0px" (only if needed, e.g. if the tool comes with default padding, you can remove it)
  - background-color="#ffffff"
- Footer section (“Disclaimer”)
  - class="footer" (to apply typography)
  - background-color="#f0f0f0" (to match body background)
  - padding-top="16px" padding-bottom="16px" (if needed?)

#### MJML (e.g. in Ruby on Rails)

If you are doing a custom software application (such as in Ruby on Rails), then MJML is one library that you might use to keep that complexity out of your email templates. Tools like MJML provide an abstraction layer over the complex markup we need to ensure cross-email-client compatibility.

You can find this in the `template_mjml.mjml` file.
There is also a compiled version you can, named `template_mjml_compiled.mjml`

The MJML approach depends on the `styles.css` file as well.

Beyond that, MJML adds additional features that aren't possible with vanilla CSS alone:

- Styling buttons that work across email clients
- Creating something that behaves as an `<hr>` element that works across email clients
- Support for a multi-column layout (not shown)

##### Running MJML Locally

To get this running locally, so you can test it:

1. Install [the MJML App](https://mjmlio.github.io/mjml-app/)
2. Download the code from this repo
3. Open the folder for this project in the MJML App

##### Running MJML Online

1. Open [the MJML online editor](https://mjml.io/try-it-live)
2. Copy-paste in the `template_mjml.mjml` file into the web editor.
3. Copy-paste the `.css` contents from `styles.css` into the same file, in the `mj-head`. Like this:

```
<mjml>
  <mj-head>
    <mj-style inline="inline">
      PASTE HERE
    </mj-style>
  </mj-head>
</mjml>
```

## Other Inspiration

This project has taken some inspiration from:

- https://caniemail.com
- https://www.goodemailcode.com/
  - In particular, the base vanilla HTML template is largely from this source.
- https://reallygoodemails.com/
- New Jersey Unemployment Insurance email templates: https://www.figma.com/community/file/1242850667740493704

## Ideas for Improvement

- We could potentially add additional components, like `alert` and others. [New Jersey Unemployment Insurance has some great examples](https://www.figma.com/community/file/1242850667740493704).
- We could share details for how to integrate this code into other platforms, such as GovDelivery and Salesforce (etc).
