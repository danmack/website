<script>
const title = "Take Screenshots";

export const resource = {
  paths: [
    "/tutorials/take-screenshots",
  ],
  meta: {
    title: title
  }
}

export default {
  components: {
  },
  data() {
    return {
      base_url: this.$conf.sinco.base_url,
      title: title,
      toc: [
        "Before You Get Started",
        "Folder Structure End State",
        "Steps",
        "Verification",
      ],
    };
  }
}
</script>

<template lang="pug">
page(
  :base_url="base_url"
  :title="title"
  :toc="toc"
)
  h2-hash Before You Get Started
  p Sinco provides the method <code>.takeScreenshot</code> that will allow you to take a screenshot of the whole page, or a specific element. This method will then write the screenshot to a file.
  p Example usage would be:
  ul
    li <code>.takeScreenshot("./screenshots");</code>
    li <code>.takeScreenshot("./screenshots", { filename: "home_page.png", selector: "body > main", format: "png" });</code>
  p The <code>.takeScreenshot</code> method will take any valid selector for <code>selector</code>.
  p If the directory for where to save the screenshot does not exist, Sinco will throw an error. By default, passing in one parameter will save a screenshot to the specified directory, where the filename is timestamped, and the format is JPEG. 
  p The method also takes an optional second parameter, which allows you to specify:
  ul
    li <code>selector</code> - If specified, Sinco will only take a screenshot of that element (and of course all of its children), as opposed to the whole page. This is useful if you might only want to see the login form, or how a single element looks.
    li <code>fileName</code> - If specified, this will be what your new image file will be named. For example, say you write <code>.takeScreenshot("./screenshots", { filename: "login_form.jpeg" })</code>, the screenshot will be saved to <code>./screenshots/login_form.jpeg</code>.
    li <code>format</code> - The format of the image, eg jpeg, png or webp. The default is jpeg.
    li <code>quality</code> - The compression quality of the screenshot. The maximum is 100, and the default is 80.
  p In this tutorial, you will:
  ul
    li Create a headless browser instance; and
    li Take a screenshot of your favourite website: drash.land!;
  hr
  folder-structure-end-state
    | ▾ /path/to/your/project/
    |     app_test.ts
  hr
  h2-hash Steps
  ol
    li
      p Create your test file.
      code-block(title="/path/to/your/project/app_test.ts" language="typescript")
        | import { buildFor } from "https://deno.land/x/sinco@{{ $conf.sinco.latest_version }}/mod.ts";
        |
        | Deno.test("My web app works as expected", async () => {
        |   const Sinco = await buildFor("chrome");
        |   await Sinco.goTo("https://drash.land");
        |   const screenshotsFolder = "./screenshots";
        |   Deno.mkdirSync(screenshotsFolder); // Ensure you create the directory your screenshots will be put within
        |   await Sinco.takeScreenshot(screenshotsFolder); // Will take a screenshot of the whole page, and write it to `./screenshots/dd_mm_yyyy_hh_mm_ss.jpeg`
        |   await Sinco.takeScreenshot(screenshotsFolder, { fileName: "drash_land.png", format: "png" }); // Specify filename and format. Will be saved as `./screenshots/drash_land.png`
        |   await Sinco.takeScreenshot(screenshotsFolder, { fileName: "modules.jpeg", selector: "#modules" }); // Will screenshot only the modules section, and write it to `./screenshots/dd_mm_yyyy_hh_mm_ss.jpeg`
        |   await Sinco.done();
        | })
      p Here you are going to create your headless browser instance, and navigate to <code>https://drash.land</code>. Once the page has loaded, you will take a screenshot of the whole page, take another screenshot with a custom filename and format, and take a third screenshot with a custom selector.
  hr
  h2-hash Verification
  ol
    li
      p Create your <code>tsconfig.json</code>. You will need this because Sinco uses DOM types to aid in creating a screenshot
      code-block(title="tsconfig.json" language="json")
        | {
        |   "compilerOptions": {
        |     "libs": ["dom", "deno.ns"]
        |   }
        | }
    li
      p Run your test.
      code-block(title="Terminal" language="shell-session")
        | $ deno test --config tsconfig.json --allow-run --allow-read --allow-write --allow-net app_test.ts
    li
      p Check your screenshots folder for the newly taken images!
</template>

