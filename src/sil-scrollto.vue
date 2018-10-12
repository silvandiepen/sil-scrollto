/* Doc.
Add the selector v-scrollto to the element you want to scroll from.
To scroll to an element, give the v-scrollto the id or class of the element to scroll to

	`<a v-scrollto="'#element-to-scroll-to">`

If you don't give a value. The directive will look for an href selector with a '#'value.

	`<a href="#element-to-scroll-to" v-scrollto></a>`

Giving extra settings like time, use comma separated values:

	`<a v-scrollto="['#element-to-scroll-to',3000,'easeInQuad'"></a>`

*/
import Vue from 'vue';
Vue.directive('scrollto', {
	bind: function(el, binding) {
		const easings = {
			linear(t) {
				return t;
			},
			easeInQuad(t) {
				return t * t;
			},
			easeOutQuad(t) {
				return t * (2 - t);
			},
			easeInOutQuad(t) {
				return t < 0.5 ? 2 * t * t : -1 + (4 - 2 * t) * t;
			},
			easeInCubic(t) {
				return t * t * t;
			},
			easeOutCubic(t) {
				return --t * t * t + 1;
			},
			easeInOutCubic(t) {
				return t < 0.5 ? 4 * t * t * t : (t - 1) * (2 * t - 2) * (2 * t - 2) + 1;
			},
			easeInQuart(t) {
				return t * t * t * t;
			},
			easeOutQuart(t) {
				return 1 - --t * t * t * t;
			},
			easeInOutQuart(t) {
				return t < 0.5 ? 8 * t * t * t * t : 1 - 8 * --t * t * t * t;
			},
			easeInQuint(t) {
				return t * t * t * t * t;
			},
			easeOutQuint(t) {
				return 1 + --t * t * t * t * t;
			},
			easeInOutQuint(t) {
				return t < 0.5 ? 16 * t * t * t * t * t : 1 + 16 * --t * t * t * t * t;
			}
		};

		let scrollIt = function(destination, duration = 200, easing = 'linear', callback) {
			const start = window.pageYOffset;
			const startTime = 'now' in window.performance ? performance.now() : new Date().getTime();

			const documentHeight = Math.max(document.body.scrollHeight, document.body.offsetHeight, document.documentElement.clientHeight, document.documentElement.scrollHeight, document.documentElement.offsetHeight);
			const windowHeight = window.innerHeight || document.documentElement.clientHeight || document.getElementsByTagName('body')[0].clientHeight;
			const destinationOffset = typeof destination === 'number' ? destination : destination.getBoundingClientRect().top;
			const destinationOffsetToScroll = Math.round(documentHeight - destinationOffset < windowHeight ? documentHeight - windowHeight : destinationOffset);

			if ('requestAnimationFrame' in window === false) {
				window.scroll(0, destinationOffsetToScroll);
				if (callback) {
					callback();
				}
				return;
			}

			function scroll() {
				const now = 'now' in window.performance ? performance.now() : new Date().getTime();
				const time = Math.min(1, (now - startTime) / duration);
				const timeFunction = easings[easing](time);
				window.scroll(0, Math.ceil(timeFunction * (destinationOffsetToScroll - start) + start));

				if (window.pageYOffset === destinationOffsetToScroll || document.body.scrollHeight === window.pageYOffset + window.innerHeight) {
					if (callback) {
						callback();
					}
					return;
				}

				requestAnimationFrame(scroll);
			}

			scroll();
		};

		// Initialize the scroll behaviour.
		el.addEventListener('click', e => {
			e.preventDefault();
			// let toSelector;
			let settings = {
				element: '',
				duration: 1000,
				easing: 'easeOutQuad'
			};
			if (Array.isArray(binding.value)) {
				settings.element = document.querySelector(binding.value[0]);
				settings.duration = binding.value[1] || 1000;
				settings.easing = binding.value[2] || 'easeOutQuad';
			} else if (binding.value) {
				settings.element = document.querySelector(binding.value);
			} else if (el.getAttribute('href')) {
				e.preventDefault();
				let href = el.getAttribute('href');
				if (href.charAt(0) == '#') {
					settings.element = document.querySelector(href);
				}
			}
			scrollIt(settings.element, settings.duration, settings.easing);
		});
	}
});
