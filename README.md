# Change slider orientation to vertical

Jet-elements uses `slider-pro` as the library for the slider, check the documentation [here](https://github.com/bqworks/slider-pro)

It has an option to change the `orientation` of the slider to `vertical`, but it is not available in the jet-elements plugin. So we need to make some changes to the script to enable it and other changes to adjust it to show it properly, these changes are documented in this repository inside `jet-elements.min.js`, you can use it to overwrite the current script on the jet-elements plugin.

Steps to manually update the script:

- Go to `Plugins > Plugin editor > JetElements > Select`

- Open `assets/js/jet-elements.min.js` file

- Change orientation to vertical and fix height to reflect the new orientation:
  - Replace `height:o,` with `orientation:'vertical',height:window.innerHeight,`

- Fix some style jump issues when the slider work for the first time:
  - Replace `find(".jet-slider__content");` with `find(".jet-slider__content");i.attr("style", 'visibility: hidden; width: 100%; height: 100%; margin: auto; inset: 0px 0px 0px 0%; transform-origin: center center; transform: scale(1); opacity: 0;');`

- Optional - Update the slider with the mouse wheel:
  - Replace `e(".slider-pro",r).sliderPro` with `(() => window.addEventListener("wheel", event => {e(".slider-pro").sliderPro(event.deltaY < 0 ? 'previousSlide' : 'nextSlide')}))();e(".slider-pro",r).sliderPro`

# Make changes available to browsers (clean cache)

- Go to `Plugins > Plugin editor > JetElements > Select`

- Open `jet-elements/includes/class-jet-elements-assets.php`

- Search and edit:

```diff
    public function enqueue_scripts() {

			$min_suffix = jet_elements_tools()->is_script_debug() ? '' : '.min';

			wp_enqueue_script(
				'jet-elements',
				jet_elements()->plugin_url( 'assets/js/jet-elements' . $min_suffix . '.js' ),
				array( 'jquery', 'elementor-frontend' ),
-				jet_elements()->get_version(),
+				jet_elements()->get_version() . 'custom', // Add 'custom' to force browsers to update the script
				true
			);

			wp_localize_script(
				'jet-elements',
				'jetElements',
				apply_filters( 'jet-elements/frontend/localize-data', $this->localize_data )
			);
		}
```
