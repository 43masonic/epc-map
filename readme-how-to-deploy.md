# Deploying a new release - GitHub Pages & Prospector

Prospector tools are hosted on GitHub Pages, each as their own repo and deployment.

# Updating an existing visualisation
Find the repo for the visualisation (you can do this by going to its page through the [SFCTA maps page](https://www.sfcta.org/tools-data/maps)). In most cases, the subdomain (the part before `.sfcta.org`) is also the repo name, so for `epc-map.sfcta.org`, the repo is `sfcta/epc-map`. This repo contains just the code and assets necessary for that specific visualisation, and is where GitHub Pages serves the app from. To update the app, the visualiser-specific repo needs to be updated.

1. Once you've found the repo, locate the `index.html` and `bundles/<visualiser-name>.js` files, these are what need to be updated.
2. Return to your updated branch of `sfcta/prospector` (likely the `develop` branch) and run `npm run prospector` or `bundle exec jekyll serve` (marginally faster) to build the bundles and html, once the build step is finished you can close out the process.
3. In your `sfcta/prospector`, find `build/bundles/<visualiser-name>.js` and `build/<visualiser-name>/index.html`. These are the files that need to jave their contents copied into the visualiser repo.
4. Copy the contents of the js bundle into the visualiser repo's `bundles/<visualiser-name>.js` file.
5. Copy the contents of the built html file into the visualiser repo's `index.html` file.
6. Edit the copied index.html and change all resource links from `//blah blah blah` to `https://blah blah blah`, and all `/assets/blah blah` to `./assets/blah blah`, ie 
    ```html
    <link rel="stylesheet" href="/assets/css/poole.css">
    <!-- BECOMES  -->
    <link rel="stylesheet" href="./assets/css/poole.css">
    ```
    and 
    ```html
    <script src='//unpkg.com/simple-statistics@7.0.2/dist/simple-statistics.min.js'></script>
    <!-- BECOMES -->
    <script src='https://unpkg.com/simple-statistics@7.0.2/dist/simple-statistics.min.js'></script>
    ```
    > **WARNING**: if you do not do step 6, the visualiser will be able to resolve any of its dependencies, scripts or styles, and will not work.

7. Ensure everything is working and linked correctly. When you open `index.html` in your browser, it should show your visualiser working properly, if things are missing or non-interactive, you probably messed up somewhere in step 6. Check the console for 404s and fix them.
8. Commit and push your changes to GitHub, then navigate the repo and click the link to view the repo's Pages deployment. If it is not up to date, make sure that step 6 was done properly and that you pushed to the branch that GitHub pages is building from.