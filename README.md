# EFK Stack on Kubernetes

This README provides a guide to setting up the EFK (Elasticsearch, Fluent Bit, Kibana) stack on a Kubernetes cluster.

## Prerequisites

* A running Kubernetes cluster.
* `kubectl` configured to interact with your cluster.
* `helm` installed.

![elk](https://github.com/user-attachments/assets/2cb0d189-87f0-4d10-ab6a-971a1b22e0fa)


[Uploading efk.html…]()<!DOCTYPE html>
<html lang="en-US" class="no-js">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<link rel="profile" href="https://gmpg.org/xfn/11">
		<link rel="pingback" href="https://opstree.com/blog/xmlrpc.php">
		<script>(function(html){html.className = html.className.replace(/\bno-js\b/,'js')})(document.documentElement);</script>
<meta name='robots' content='index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1' />
<!-- Jetpack Site Verification Tags -->
<!-- Google tag (gtag.js) Consent Mode snippet added by Site Kit -->
<script id='google_gtagjs-js-consent-mode'>
window.dataLayer = window.dataLayer || [];function gtag(){dataLayer.push(arguments);}
gtag('consent', 'default', {"ad_personalization":"denied","ad_storage":"denied","ad_user_data":"denied","analytics_storage":"denied","region":["AT","BE","BG","CY","CZ","DE","DK","EE","ES","FI","FR","GB","GR","HR","HU","IE","IS","IT","LI","LT","LU","LV","MT","NL","NO","PL","PT","RO","SE","SI","SK"],"wait_for_update":500});
window._googlesitekitConsentCategoryMap = {"statistics":["analytics_storage"],"marketing":["ad_storage","ad_user_data","ad_personalization"]};
( function () {
	document.addEventListener(
		'wp_listen_for_consent_change',
		function ( event ) {
			if ( event.detail ) {
				var consentParameters = {};
				var hasConsentParameters = false;
				for ( var category in event.detail ) {
					if ( window._googlesitekitConsentCategoryMap[ category ] ) {
						var status = event.detail[ category ];
						var mappedStatus =
							status === 'allow' ? 'granted' : 'denied';
						var parameters =
							window._googlesitekitConsentCategoryMap[ category ];
						for ( var i = 0; i < parameters.length; i++ ) {
							consentParameters[ parameters[ i ] ] = mappedStatus;
						}
						hasConsentParameters = !! parameters.length;
					}
				}
				if ( hasConsentParameters ) {
					gtag( 'consent', 'update', consentParameters );
				}
			}
		}
	);

	function updateGrantedConsent() {
		if ( ! ( window.wp_consent_type || window.wp_fallback_consent_type ) ) {
			return;
		}
		var consentParameters = {};
		var hasConsentParameters = false;
		for ( var category in window._googlesitekitConsentCategoryMap ) {
			if ( window.wp_has_consent && window.wp_has_consent( category ) ) {
				var parameters =
					window._googlesitekitConsentCategoryMap[ category ];
				for ( var i = 0; i < parameters.length; i++ ) {
					consentParameters[ parameters[ i ] ] = 'granted';
				}
				hasConsentParameters =
					hasConsentParameters || !! parameters.length;
			}
		}
		if ( hasConsentParameters ) {
			gtag( 'consent', 'update', consentParameters );
		}
	}
	document.addEventListener(
		'wp_consent_type_defined',
		updateGrantedConsent
	);
	document.addEventListener( 'DOMContentLoaded', function () {
		if ( ! window.waitfor_consent_hook ) {
			updateGrantedConsent();
		}
	} );
} )();
</script>
<!-- End Google tag (gtag.js) Consent Mode snippet added by Site Kit -->
			
	<!-- This site is optimized with the Yoast SEO plugin v22.5 - https://yoast.com/wordpress/plugins/seo/ -->
	<title>Protected EFK Stack Setup for Kubernetes - DEVOPS DONE RIGHT.</title>
	<link rel="canonical" href="https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/" />
	<meta property="og:locale" content="en_US" />
	<meta property="og:type" content="article" />
	<meta property="og:title" content="Protected EFK Stack Setup for Kubernetes - DEVOPS DONE RIGHT." />
	<meta property="og:description" content="In this blog, we will see how we can deploy the Elasticsearch, Fluent-bit, and Kibana (EFK) stack on Kubernetes. EFK stack’s prime objective is to reliably and securely retrieve data from the K8s cluster in any format, as well as to facilitate anytime searching, analyzing, and visualizing of the data. What is EFK&nbsp;Stack? EFK stands &hellip; Continue reading &quot;Protected EFK Stack Setup for Kubernetes&quot;" />
	<meta property="og:url" content="https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/" />
	<meta property="og:site_name" content="DEVOPS DONE RIGHT." />
	<meta property="article:published_time" content="2023-01-24T06:23:29+00:00" />
	<meta property="article:modified_time" content="2023-08-21T05:55:58+00:00" />
	<meta property="og:image" content="https://opstree.com/blog//wp-content/uploads/2023/01/fd618-1j4l3cowyx0orfe0qqiodbq.png" />
	<meta name="author" content="Priyanshi Chauhan" />
	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:creator" content="@opstreedevops" />
	<meta name="twitter:site" content="@opstreedevops" />
	<meta name="twitter:label1" content="Written by" />
	<meta name="twitter:data1" content="Priyanshi Chauhan" />
	<meta name="twitter:label2" content="Est. reading time" />
	<meta name="twitter:data2" content="10 minutes" />
	<script type="application/ld+json" class="yoast-schema-graph">{"@context":"https://schema.org","@graph":[{"@type":"Article","@id":"https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/#article","isPartOf":{"@id":"https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/"},"author":{"name":"Priyanshi Chauhan","@id":"https://opstree.com/blog/#/schema/person/d9b3f47c065b342b10d31aeaa7f769b1"},"headline":"Protected EFK Stack Setup for Kubernetes","datePublished":"2023-01-24T06:23:29+00:00","dateModified":"2023-08-21T05:55:58+00:00","mainEntityOfPage":{"@id":"https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/"},"wordCount":1888,"commentCount":2,"publisher":{"@id":"https://opstree.com/blog/#organization"},"image":{"@id":"https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/#primaryimage"},"thumbnailUrl":"https://opstree.com/blog//wp-content/uploads/2023/01/fd618-1j4l3cowyx0orfe0qqiodbq.png","keywords":["Automation","DevOps","Devops Solutioning","EFK","Kubernetes","Monitoring","technical blogs"],"articleSection":["DevOps"],"inLanguage":"en-US","potentialAction":[{"@type":"CommentAction","name":"Comment","target":["https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/#respond"]}]},{"@type":"WebPage","@id":"https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/","url":"https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/","name":"Protected EFK Stack Setup for Kubernetes - DEVOPS DONE RIGHT.","isPartOf":{"@id":"https://opstree.com/blog/#website"},"primaryImageOfPage":{"@id":"https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/#primaryimage"},"image":{"@id":"https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/#primaryimage"},"thumbnailUrl":"https://opstree.com/blog//wp-content/uploads/2023/01/fd618-1j4l3cowyx0orfe0qqiodbq.png","datePublished":"2023-01-24T06:23:29+00:00","dateModified":"2023-08-21T05:55:58+00:00","breadcrumb":{"@id":"https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/#breadcrumb"},"inLanguage":"en-US","potentialAction":[{"@type":"ReadAction","target":["https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/"]}]},{"@type":"ImageObject","inLanguage":"en-US","@id":"https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/#primaryimage","url":"https://opstree.com/blog//wp-content/uploads/2023/01/fd618-1j4l3cowyx0orfe0qqiodbq.png","contentUrl":"https://opstree.com/blog//wp-content/uploads/2023/01/fd618-1j4l3cowyx0orfe0qqiodbq.png"},{"@type":"BreadcrumbList","@id":"https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/#breadcrumb","itemListElement":[{"@type":"ListItem","position":1,"name":"Home","item":"https://opstree.com/blog/"},{"@type":"ListItem","position":2,"name":"Protected EFK Stack Setup for Kubernetes"}]},{"@type":"WebSite","@id":"https://opstree.com/blog/#website","url":"https://opstree.com/blog/","name":"DEVOPS DONE RIGHT","description":"A blog site on our Real life experiences with various phases of DevOps starting from VCS, Build &amp; Release, CI/CD, Cloud, Monitoring, Containerization.","publisher":{"@id":"https://opstree.com/blog/#organization"},"potentialAction":[{"@type":"SearchAction","target":{"@type":"EntryPoint","urlTemplate":"https://opstree.com/blog/?s={search_term_string}"},"query-input":"required name=search_term_string"}],"inLanguage":"en-US"},{"@type":"Organization","@id":"https://opstree.com/blog/#organization","name":"DEVOPS DONE RIGHT","url":"https://opstree.com/blog/","logo":{"@type":"ImageObject","inLanguage":"en-US","@id":"https://opstree.com/blog/#/schema/logo/image/","url":"https://i0.wp.com/opstree.com/blog/wp-content/uploads/2020/08/logo-black.png?fit=203%2C41&ssl=1","contentUrl":"https://i0.wp.com/opstree.com/blog/wp-content/uploads/2020/08/logo-black.png?fit=203%2C41&ssl=1","width":203,"height":41,"caption":"DEVOPS DONE RIGHT"},"image":{"@id":"https://opstree.com/blog/#/schema/logo/image/"},"sameAs":["https://x.com/opstreedevops","https://in.linkedin.com/company/opstree-solutions"]},{"@type":"Person","@id":"https://opstree.com/blog/#/schema/person/d9b3f47c065b342b10d31aeaa7f769b1","name":"Priyanshi Chauhan","image":{"@type":"ImageObject","inLanguage":"en-US","@id":"https://opstree.com/blog/#/schema/person/image/","url":"https://secure.gravatar.com/avatar/863b5758f40b7e44062b3a48738d7d12?s=96&d=identicon&r=g","contentUrl":"https://secure.gravatar.com/avatar/863b5758f40b7e44062b3a48738d7d12?s=96&d=identicon&r=g","caption":"Priyanshi Chauhan"},"description":"I am a DevOps Engineer","url":"https://opstree.com/blog/author/priyanshi0707/"}]}</script>
	<!-- / Yoast SEO plugin. -->


<link rel='dns-prefetch' href='//secure.gravatar.com' />
<link rel='dns-prefetch' href='//www.googletagmanager.com' />
<link rel='dns-prefetch' href='//stats.wp.com' />
<link rel='dns-prefetch' href='//widgets.wp.com' />
<link rel='dns-prefetch' href='//jetpack.wordpress.com' />
<link rel='dns-prefetch' href='//s0.wp.com' />
<link rel='dns-prefetch' href='//public-api.wordpress.com' />
<link rel='dns-prefetch' href='//0.gravatar.com' />
<link rel='dns-prefetch' href='//1.gravatar.com' />
<link rel='dns-prefetch' href='//2.gravatar.com' />
<link rel='dns-prefetch' href='//i0.wp.com' />
<link rel='dns-prefetch' href='//c0.wp.com' />
<link rel="alternate" type="application/rss+xml" title="DEVOPS DONE RIGHT. &raquo; Feed" href="https://opstree.com/blog/feed/" />
<link rel="alternate" type="application/rss+xml" title="DEVOPS DONE RIGHT. &raquo; Comments Feed" href="https://opstree.com/blog/comments/feed/" />
<link rel="alternate" type="application/rss+xml" title="DEVOPS DONE RIGHT. &raquo; Protected EFK Stack Setup for Kubernetes Comments Feed" href="https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/feed/" />
<script>
window._wpemojiSettings = {"baseUrl":"https:\/\/s.w.org\/images\/core\/emoji\/15.0.3\/72x72\/","ext":".png","svgUrl":"https:\/\/s.w.org\/images\/core\/emoji\/15.0.3\/svg\/","svgExt":".svg","source":{"concatemoji":"https:\/\/opstree.com\/blog\/wp-includes\/js\/wp-emoji-release.min.js?ver=6.5.5"}};
/*! This file is auto-generated */
!function(i,n){var o,s,e;function c(e){try{var t={supportTests:e,timestamp:(new Date).valueOf()};sessionStorage.setItem(o,JSON.stringify(t))}catch(e){}}function p(e,t,n){e.clearRect(0,0,e.canvas.width,e.canvas.height),e.fillText(t,0,0);var t=new Uint32Array(e.getImageData(0,0,e.canvas.width,e.canvas.height).data),r=(e.clearRect(0,0,e.canvas.width,e.canvas.height),e.fillText(n,0,0),new Uint32Array(e.getImageData(0,0,e.canvas.width,e.canvas.height).data));return t.every(function(e,t){return e===r[t]})}function u(e,t,n){switch(t){case"flag":return n(e,"\ud83c\udff3\ufe0f\u200d\u26a7\ufe0f","\ud83c\udff3\ufe0f\u200b\u26a7\ufe0f")?!1:!n(e,"\ud83c\uddfa\ud83c\uddf3","\ud83c\uddfa\u200b\ud83c\uddf3")&&!n(e,"\ud83c\udff4\udb40\udc67\udb40\udc62\udb40\udc65\udb40\udc6e\udb40\udc67\udb40\udc7f","\ud83c\udff4\u200b\udb40\udc67\u200b\udb40\udc62\u200b\udb40\udc65\u200b\udb40\udc6e\u200b\udb40\udc67\u200b\udb40\udc7f");case"emoji":return!n(e,"\ud83d\udc26\u200d\u2b1b","\ud83d\udc26\u200b\u2b1b")}return!1}function f(e,t,n){var r="undefined"!=typeof WorkerGlobalScope&&self instanceof WorkerGlobalScope?new OffscreenCanvas(300,150):i.createElement("canvas"),a=r.getContext("2d",{willReadFrequently:!0}),o=(a.textBaseline="top",a.font="600 32px Arial",{});return e.forEach(function(e){o[e]=t(a,e,n)}),o}function t(e){var t=i.createElement("script");t.src=e,t.defer=!0,i.head.appendChild(t)}"undefined"!=typeof Promise&&(o="wpEmojiSettingsSupports",s=["flag","emoji"],n.supports={everything:!0,everythingExceptFlag:!0},e=new Promise(function(e){i.addEventListener("DOMContentLoaded",e,{once:!0})}),new Promise(function(t){var n=function(){try{var e=JSON.parse(sessionStorage.getItem(o));if("object"==typeof e&&"number"==typeof e.timestamp&&(new Date).valueOf()<e.timestamp+604800&&"object"==typeof e.supportTests)return e.supportTests}catch(e){}return null}();if(!n){if("undefined"!=typeof Worker&&"undefined"!=typeof OffscreenCanvas&&"undefined"!=typeof URL&&URL.createObjectURL&&"undefined"!=typeof Blob)try{var e="postMessage("+f.toString()+"("+[JSON.stringify(s),u.toString(),p.toString()].join(",")+"));",r=new Blob([e],{type:"text/javascript"}),a=new Worker(URL.createObjectURL(r),{name:"wpTestEmojiSupports"});return void(a.onmessage=function(e){c(n=e.data),a.terminate(),t(n)})}catch(e){}c(n=f(s,u,p))}t(n)}).then(function(e){for(var t in e)n.supports[t]=e[t],n.supports.everything=n.supports.everything&&n.supports[t],"flag"!==t&&(n.supports.everythingExceptFlag=n.supports.everythingExceptFlag&&n.supports[t]);n.supports.everythingExceptFlag=n.supports.everythingExceptFlag&&!n.supports.flag,n.DOMReady=!1,n.readyCallback=function(){n.DOMReady=!0}}).then(function(){return e}).then(function(){var e;n.supports.everything||(n.readyCallback(),(e=n.source||{}).concatemoji?t(e.concatemoji):e.wpemoji&&e.twemoji&&(t(e.twemoji),t(e.wpemoji)))}))}((window,document),window._wpemojiSettings);
</script>
<link rel='stylesheet' id='twentysixteen-jetpack-css' href='https://c0.wp.com/p/jetpack/13.3.2/modules/theme-tools/compat/twentysixteen.css' media='all' />
<!-- `jetpack_related-posts` is included in the concatenated jetpack.css -->
<style id='wp-emoji-styles-inline-css'>

	img.wp-smiley, img.emoji {
		display: inline !important;
		border: none !important;
		box-shadow: none !important;
		height: 1em !important;
		width: 1em !important;
		margin: 0 0.07em !important;
		vertical-align: -0.1em !important;
		background: none !important;
		padding: 0 !important;
	}
</style>
<link rel='stylesheet' id='wp-block-library-css' href='https://opstree.com/blog/wp-content/plugins/gutenberg/build/block-library/style.css?ver=17.9.0' media='all' />
<style id='wp-block-library-inline-css'>
.has-text-align-justify{text-align:justify;}
</style>
<link rel='stylesheet' id='wp-block-library-theme-css' href='https://opstree.com/blog/wp-content/plugins/gutenberg/build/block-library/theme.css?ver=17.9.0' media='all' />
<link rel='stylesheet' id='mediaelement-css' href='https://c0.wp.com/c/6.5.5/wp-includes/js/mediaelement/mediaelementplayer-legacy.min.css' media='all' />
<link rel='stylesheet' id='wp-mediaelement-css' href='https://c0.wp.com/c/6.5.5/wp-includes/js/mediaelement/wp-mediaelement.min.css' media='all' />
<style id='jetpack-sharing-buttons-style-inline-css'>
.jetpack-sharing-buttons__services-list{display:flex;flex-direction:row;flex-wrap:wrap;gap:0;list-style-type:none;margin:5px;padding:0}.jetpack-sharing-buttons__services-list.has-small-icon-size{font-size:12px}.jetpack-sharing-buttons__services-list.has-normal-icon-size{font-size:16px}.jetpack-sharing-buttons__services-list.has-large-icon-size{font-size:24px}.jetpack-sharing-buttons__services-list.has-huge-icon-size{font-size:36px}@media print{.jetpack-sharing-buttons__services-list{display:none!important}}.editor-styles-wrapper .wp-block-jetpack-sharing-buttons{gap:0;padding-inline-start:0}ul.jetpack-sharing-buttons__services-list.has-background{padding:1.25em 2.375em}
</style>
<link rel='stylesheet' id='coblocks-frontend-css' href='https://opstree.com/blog/wp-content/plugins/coblocks/dist/style-coblocks-1.css?ver=3.1.7' media='all' />
<link rel='stylesheet' id='coblocks-extensions-css' href='https://opstree.com/blog/wp-content/plugins/coblocks/dist/style-coblocks-extensions.css?ver=3.1.7' media='all' />
<link rel='stylesheet' id='coblocks-animation-css' href='https://opstree.com/blog/wp-content/plugins/coblocks/dist/style-coblocks-animation.css?ver=d9b2b27566e6a2a85d1b' media='all' />
<style id='classic-theme-styles-inline-css'>
/*! This file is auto-generated */
.wp-block-button__link{color:#fff;background-color:#32373c;border-radius:9999px;box-shadow:none;text-decoration:none;padding:calc(.667em + 2px) calc(1.333em + 2px);font-size:1.125em}.wp-block-file__button{background:#32373c;color:#fff;text-decoration:none}
</style>
<style id='global-styles-inline-css'>
body{--wp--preset--color--black: #000000;--wp--preset--color--cyan-bluish-gray: #abb8c3;--wp--preset--color--white: #fff;--wp--preset--color--pale-pink: #f78da7;--wp--preset--color--vivid-red: #cf2e2e;--wp--preset--color--luminous-vivid-orange: #ff6900;--wp--preset--color--luminous-vivid-amber: #fcb900;--wp--preset--color--light-green-cyan: #7bdcb5;--wp--preset--color--vivid-green-cyan: #00d084;--wp--preset--color--pale-cyan-blue: #8ed1fc;--wp--preset--color--vivid-cyan-blue: #0693e3;--wp--preset--color--vivid-purple: #9b51e0;--wp--preset--color--dark-gray: #1a1a1a;--wp--preset--color--medium-gray: #686868;--wp--preset--color--light-gray: #e5e5e5;--wp--preset--color--blue-gray: #4d545c;--wp--preset--color--bright-blue: #007acc;--wp--preset--color--light-blue: #9adffd;--wp--preset--color--dark-brown: #402b30;--wp--preset--color--medium-brown: #774e24;--wp--preset--color--dark-red: #640c1f;--wp--preset--color--bright-red: #ff675f;--wp--preset--color--yellow: #ffef8e;--wp--preset--gradient--vivid-cyan-blue-to-vivid-purple: linear-gradient(135deg,rgba(6,147,227,1) 0%,rgb(155,81,224) 100%);--wp--preset--gradient--light-green-cyan-to-vivid-green-cyan: linear-gradient(135deg,rgb(122,220,180) 0%,rgb(0,208,130) 100%);--wp--preset--gradient--luminous-vivid-amber-to-luminous-vivid-orange: linear-gradient(135deg,rgba(252,185,0,1) 0%,rgba(255,105,0,1) 100%);--wp--preset--gradient--luminous-vivid-orange-to-vivid-red: linear-gradient(135deg,rgba(255,105,0,1) 0%,rgb(207,46,46) 100%);--wp--preset--gradient--very-light-gray-to-cyan-bluish-gray: linear-gradient(135deg,rgb(238,238,238) 0%,rgb(169,184,195) 100%);--wp--preset--gradient--cool-to-warm-spectrum: linear-gradient(135deg,rgb(74,234,220) 0%,rgb(151,120,209) 20%,rgb(207,42,186) 40%,rgb(238,44,130) 60%,rgb(251,105,98) 80%,rgb(254,248,76) 100%);--wp--preset--gradient--blush-light-purple: linear-gradient(135deg,rgb(255,206,236) 0%,rgb(152,150,240) 100%);--wp--preset--gradient--blush-bordeaux: linear-gradient(135deg,rgb(254,205,165) 0%,rgb(254,45,45) 50%,rgb(107,0,62) 100%);--wp--preset--gradient--luminous-dusk: linear-gradient(135deg,rgb(255,203,112) 0%,rgb(199,81,192) 50%,rgb(65,88,208) 100%);--wp--preset--gradient--pale-ocean: linear-gradient(135deg,rgb(255,245,203) 0%,rgb(182,227,212) 50%,rgb(51,167,181) 100%);--wp--preset--gradient--electric-grass: linear-gradient(135deg,rgb(202,248,128) 0%,rgb(113,206,126) 100%);--wp--preset--gradient--midnight: linear-gradient(135deg,rgb(2,3,129) 0%,rgb(40,116,252) 100%);--wp--preset--font-size--small: 13px;--wp--preset--font-size--medium: 20px;--wp--preset--font-size--large: 36px;--wp--preset--font-size--x-large: 42px;--wp--preset--font-family--albert-sans: 'Albert Sans', sans-serif;--wp--preset--font-family--alegreya: Alegreya, serif;--wp--preset--font-family--arvo: Arvo, serif;--wp--preset--font-family--bodoni-moda: 'Bodoni Moda', serif;--wp--preset--font-family--bricolage-grotesque: 'Bricolage Grotesque', sans-serif;--wp--preset--font-family--cabin: Cabin, sans-serif;--wp--preset--font-family--chivo: Chivo, sans-serif;--wp--preset--font-family--commissioner: Commissioner, sans-serif;--wp--preset--font-family--cormorant: Cormorant, serif;--wp--preset--font-family--courier-prime: 'Courier Prime', monospace;--wp--preset--font-family--crimson-pro: 'Crimson Pro', serif;--wp--preset--font-family--dm-mono: 'DM Mono', monospace;--wp--preset--font-family--dm-sans: 'DM Sans', sans-serif;--wp--preset--font-family--dm-serif-display: 'DM Serif Display', serif;--wp--preset--font-family--domine: Domine, serif;--wp--preset--font-family--eb-garamond: 'EB Garamond', serif;--wp--preset--font-family--epilogue: Epilogue, sans-serif;--wp--preset--font-family--fahkwang: Fahkwang, sans-serif;--wp--preset--font-family--figtree: Figtree, sans-serif;--wp--preset--font-family--fira-sans: 'Fira Sans', sans-serif;--wp--preset--font-family--fjalla-one: 'Fjalla One', sans-serif;--wp--preset--font-family--fraunces: Fraunces, serif;--wp--preset--font-family--gabarito: Gabarito, system-ui;--wp--preset--font-family--ibm-plex-mono: 'IBM Plex Mono', monospace;--wp--preset--font-family--ibm-plex-sans: 'IBM Plex Sans', sans-serif;--wp--preset--font-family--ibarra-real-nova: 'Ibarra Real Nova', serif;--wp--preset--font-family--instrument-serif: 'Instrument Serif', serif;--wp--preset--font-family--inter: Inter, sans-serif;--wp--preset--font-family--josefin-sans: 'Josefin Sans', sans-serif;--wp--preset--font-family--jost: Jost, sans-serif;--wp--preset--font-family--libre-baskerville: 'Libre Baskerville', serif;--wp--preset--font-family--libre-franklin: 'Libre Franklin', sans-serif;--wp--preset--font-family--literata: Literata, serif;--wp--preset--font-family--lora: Lora, serif;--wp--preset--font-family--merriweather: Merriweather, serif;--wp--preset--font-family--montserrat: Montserrat, sans-serif;--wp--preset--font-family--newsreader: Newsreader, serif;--wp--preset--font-family--noto-sans-mono: 'Noto Sans Mono', sans-serif;--wp--preset--font-family--nunito: Nunito, sans-serif;--wp--preset--font-family--open-sans: 'Open Sans', sans-serif;--wp--preset--font-family--overpass: Overpass, sans-serif;--wp--preset--font-family--pt-serif: 'PT Serif', serif;--wp--preset--font-family--petrona: Petrona, serif;--wp--preset--font-family--piazzolla: Piazzolla, serif;--wp--preset--font-family--playfair-display: 'Playfair Display', serif;--wp--preset--font-family--plus-jakarta-sans: 'Plus Jakarta Sans', sans-serif;--wp--preset--font-family--poppins: Poppins, sans-serif;--wp--preset--font-family--raleway: Raleway, sans-serif;--wp--preset--font-family--roboto: Roboto, sans-serif;--wp--preset--font-family--roboto-slab: 'Roboto Slab', serif;--wp--preset--font-family--rubik: Rubik, sans-serif;--wp--preset--font-family--rufina: Rufina, serif;--wp--preset--font-family--sora: Sora, sans-serif;--wp--preset--font-family--source-sans-3: 'Source Sans 3', sans-serif;--wp--preset--font-family--source-serif-4: 'Source Serif 4', serif;--wp--preset--font-family--space-mono: 'Space Mono', monospace;--wp--preset--font-family--syne: Syne, sans-serif;--wp--preset--font-family--texturina: Texturina, serif;--wp--preset--font-family--urbanist: Urbanist, sans-serif;--wp--preset--font-family--work-sans: 'Work Sans', sans-serif;--wp--preset--spacing--20: 0.44rem;--wp--preset--spacing--30: 0.67rem;--wp--preset--spacing--40: 1rem;--wp--preset--spacing--50: 1.5rem;--wp--preset--spacing--60: 2.25rem;--wp--preset--spacing--70: 3.38rem;--wp--preset--spacing--80: 5.06rem;--wp--preset--shadow--natural: 6px 6px 9px rgba(0, 0, 0, 0.2);--wp--preset--shadow--deep: 12px 12px 50px rgba(0, 0, 0, 0.4);--wp--preset--shadow--sharp: 6px 6px 0px rgba(0, 0, 0, 0.2);--wp--preset--shadow--outlined: 6px 6px 0px -3px rgba(255, 255, 255, 1), 6px 6px rgba(0, 0, 0, 1);--wp--preset--shadow--crisp: 6px 6px 0px rgba(0, 0, 0, 1);}:where(.is-layout-flex){gap: 0.5em;}:where(.is-layout-grid){gap: 0.5em;}body .is-layout-flow > .alignleft{float: left;margin-inline-start: 0;margin-inline-end: 2em;}body .is-layout-flow > .alignright{float: right;margin-inline-start: 2em;margin-inline-end: 0;}body .is-layout-flow > .aligncenter{margin-left: auto !important;margin-right: auto !important;}body .is-layout-constrained > .alignleft{float: left;margin-inline-start: 0;margin-inline-end: 2em;}body .is-layout-constrained > .alignright{float: right;margin-inline-start: 2em;margin-inline-end: 0;}body .is-layout-constrained > .aligncenter{margin-left: auto !important;margin-right: auto !important;}body .is-layout-constrained > :where(:not(.alignleft):not(.alignright):not(.alignfull)){max-width: var(--wp--style--global--content-size);margin-left: auto !important;margin-right: auto !important;}body .is-layout-constrained > .alignwide{max-width: var(--wp--style--global--wide-size);}body .is-layout-flex{display: flex;}body .is-layout-flex{flex-wrap: wrap;align-items: center;}body .is-layout-flex > *{margin: 0;}body .is-layout-grid{display: grid;}body .is-layout-grid > *{margin: 0;}:where(.wp-block-columns.is-layout-flex){gap: 2em;}:where(.wp-block-columns.is-layout-grid){gap: 2em;}:where(.wp-block-post-template.is-layout-flex){gap: 1.25em;}:where(.wp-block-post-template.is-layout-grid){gap: 1.25em;}.has-black-color{color: var(--wp--preset--color--black) !important;}.has-cyan-bluish-gray-color{color: var(--wp--preset--color--cyan-bluish-gray) !important;}.has-white-color{color: var(--wp--preset--color--white) !important;}.has-pale-pink-color{color: var(--wp--preset--color--pale-pink) !important;}.has-vivid-red-color{color: var(--wp--preset--color--vivid-red) !important;}.has-luminous-vivid-orange-color{color: var(--wp--preset--color--luminous-vivid-orange) !important;}.has-luminous-vivid-amber-color{color: var(--wp--preset--color--luminous-vivid-amber) !important;}.has-light-green-cyan-color{color: var(--wp--preset--color--light-green-cyan) !important;}.has-vivid-green-cyan-color{color: var(--wp--preset--color--vivid-green-cyan) !important;}.has-pale-cyan-blue-color{color: var(--wp--preset--color--pale-cyan-blue) !important;}.has-vivid-cyan-blue-color{color: var(--wp--preset--color--vivid-cyan-blue) !important;}.has-vivid-purple-color{color: var(--wp--preset--color--vivid-purple) !important;}.has-black-background-color{background-color: var(--wp--preset--color--black) !important;}.has-cyan-bluish-gray-background-color{background-color: var(--wp--preset--color--cyan-bluish-gray) !important;}.has-white-background-color{background-color: var(--wp--preset--color--white) !important;}.has-pale-pink-background-color{background-color: var(--wp--preset--color--pale-pink) !important;}.has-vivid-red-background-color{background-color: var(--wp--preset--color--vivid-red) !important;}.has-luminous-vivid-orange-background-color{background-color: var(--wp--preset--color--luminous-vivid-orange) !important;}.has-luminous-vivid-amber-background-color{background-color: var(--wp--preset--color--luminous-vivid-amber) !important;}.has-light-green-cyan-background-color{background-color: var(--wp--preset--color--light-green-cyan) !important;}.has-vivid-green-cyan-background-color{background-color: var(--wp--preset--color--vivid-green-cyan) !important;}.has-pale-cyan-blue-background-color{background-color: var(--wp--preset--color--pale-cyan-blue) !important;}.has-vivid-cyan-blue-background-color{background-color: var(--wp--preset--color--vivid-cyan-blue) !important;}.has-vivid-purple-background-color{background-color: var(--wp--preset--color--vivid-purple) !important;}.has-black-border-color{border-color: var(--wp--preset--color--black) !important;}.has-cyan-bluish-gray-border-color{border-color: var(--wp--preset--color--cyan-bluish-gray) !important;}.has-white-border-color{border-color: var(--wp--preset--color--white) !important;}.has-pale-pink-border-color{border-color: var(--wp--preset--color--pale-pink) !important;}.has-vivid-red-border-color{border-color: var(--wp--preset--color--vivid-red) !important;}.has-luminous-vivid-orange-border-color{border-color: var(--wp--preset--color--luminous-vivid-orange) !important;}.has-luminous-vivid-amber-border-color{border-color: var(--wp--preset--color--luminous-vivid-amber) !important;}.has-light-green-cyan-border-color{border-color: var(--wp--preset--color--light-green-cyan) !important;}.has-vivid-green-cyan-border-color{border-color: var(--wp--preset--color--vivid-green-cyan) !important;}.has-pale-cyan-blue-border-color{border-color: var(--wp--preset--color--pale-cyan-blue) !important;}.has-vivid-cyan-blue-border-color{border-color: var(--wp--preset--color--vivid-cyan-blue) !important;}.has-vivid-purple-border-color{border-color: var(--wp--preset--color--vivid-purple) !important;}.has-vivid-cyan-blue-to-vivid-purple-gradient-background{background: var(--wp--preset--gradient--vivid-cyan-blue-to-vivid-purple) !important;}.has-light-green-cyan-to-vivid-green-cyan-gradient-background{background: var(--wp--preset--gradient--light-green-cyan-to-vivid-green-cyan) !important;}.has-luminous-vivid-amber-to-luminous-vivid-orange-gradient-background{background: var(--wp--preset--gradient--luminous-vivid-amber-to-luminous-vivid-orange) !important;}.has-luminous-vivid-orange-to-vivid-red-gradient-background{background: var(--wp--preset--gradient--luminous-vivid-orange-to-vivid-red) !important;}.has-very-light-gray-to-cyan-bluish-gray-gradient-background{background: var(--wp--preset--gradient--very-light-gray-to-cyan-bluish-gray) !important;}.has-cool-to-warm-spectrum-gradient-background{background: var(--wp--preset--gradient--cool-to-warm-spectrum) !important;}.has-blush-light-purple-gradient-background{background: var(--wp--preset--gradient--blush-light-purple) !important;}.has-blush-bordeaux-gradient-background{background: var(--wp--preset--gradient--blush-bordeaux) !important;}.has-luminous-dusk-gradient-background{background: var(--wp--preset--gradient--luminous-dusk) !important;}.has-pale-ocean-gradient-background{background: var(--wp--preset--gradient--pale-ocean) !important;}.has-electric-grass-gradient-background{background: var(--wp--preset--gradient--electric-grass) !important;}.has-midnight-gradient-background{background: var(--wp--preset--gradient--midnight) !important;}.has-small-font-size{font-size: var(--wp--preset--font-size--small) !important;}.has-medium-font-size{font-size: var(--wp--preset--font-size--medium) !important;}.has-large-font-size{font-size: var(--wp--preset--font-size--large) !important;}.has-x-large-font-size{font-size: var(--wp--preset--font-size--x-large) !important;}.has-albert-sans-font-family{font-family: var(--wp--preset--font-family--albert-sans) !important;}.has-alegreya-font-family{font-family: var(--wp--preset--font-family--alegreya) !important;}.has-arvo-font-family{font-family: var(--wp--preset--font-family--arvo) !important;}.has-bodoni-moda-font-family{font-family: var(--wp--preset--font-family--bodoni-moda) !important;}.has-bricolage-grotesque-font-family{font-family: var(--wp--preset--font-family--bricolage-grotesque) !important;}.has-cabin-font-family{font-family: var(--wp--preset--font-family--cabin) !important;}.has-chivo-font-family{font-family: var(--wp--preset--font-family--chivo) !important;}.has-commissioner-font-family{font-family: var(--wp--preset--font-family--commissioner) !important;}.has-cormorant-font-family{font-family: var(--wp--preset--font-family--cormorant) !important;}.has-courier-prime-font-family{font-family: var(--wp--preset--font-family--courier-prime) !important;}.has-crimson-pro-font-family{font-family: var(--wp--preset--font-family--crimson-pro) !important;}.has-dm-mono-font-family{font-family: var(--wp--preset--font-family--dm-mono) !important;}.has-dm-sans-font-family{font-family: var(--wp--preset--font-family--dm-sans) !important;}.has-dm-serif-display-font-family{font-family: var(--wp--preset--font-family--dm-serif-display) !important;}.has-domine-font-family{font-family: var(--wp--preset--font-family--domine) !important;}.has-eb-garamond-font-family{font-family: var(--wp--preset--font-family--eb-garamond) !important;}.has-epilogue-font-family{font-family: var(--wp--preset--font-family--epilogue) !important;}.has-fahkwang-font-family{font-family: var(--wp--preset--font-family--fahkwang) !important;}.has-figtree-font-family{font-family: var(--wp--preset--font-family--figtree) !important;}.has-fira-sans-font-family{font-family: var(--wp--preset--font-family--fira-sans) !important;}.has-fjalla-one-font-family{font-family: var(--wp--preset--font-family--fjalla-one) !important;}.has-fraunces-font-family{font-family: var(--wp--preset--font-family--fraunces) !important;}.has-gabarito-font-family{font-family: var(--wp--preset--font-family--gabarito) !important;}.has-ibm-plex-mono-font-family{font-family: var(--wp--preset--font-family--ibm-plex-mono) !important;}.has-ibm-plex-sans-font-family{font-family: var(--wp--preset--font-family--ibm-plex-sans) !important;}.has-ibarra-real-nova-font-family{font-family: var(--wp--preset--font-family--ibarra-real-nova) !important;}.has-instrument-serif-font-family{font-family: var(--wp--preset--font-family--instrument-serif) !important;}.has-inter-font-family{font-family: var(--wp--preset--font-family--inter) !important;}.has-josefin-sans-font-family{font-family: var(--wp--preset--font-family--josefin-sans) !important;}.has-jost-font-family{font-family: var(--wp--preset--font-family--jost) !important;}.has-libre-baskerville-font-family{font-family: var(--wp--preset--font-family--libre-baskerville) !important;}.has-libre-franklin-font-family{font-family: var(--wp--preset--font-family--libre-franklin) !important;}.has-literata-font-family{font-family: var(--wp--preset--font-family--literata) !important;}.has-lora-font-family{font-family: var(--wp--preset--font-family--lora) !important;}.has-merriweather-font-family{font-family: var(--wp--preset--font-family--merriweather) !important;}.has-montserrat-font-family{font-family: var(--wp--preset--font-family--montserrat) !important;}.has-newsreader-font-family{font-family: var(--wp--preset--font-family--newsreader) !important;}.has-noto-sans-mono-font-family{font-family: var(--wp--preset--font-family--noto-sans-mono) !important;}.has-nunito-font-family{font-family: var(--wp--preset--font-family--nunito) !important;}.has-open-sans-font-family{font-family: var(--wp--preset--font-family--open-sans) !important;}.has-overpass-font-family{font-family: var(--wp--preset--font-family--overpass) !important;}.has-pt-serif-font-family{font-family: var(--wp--preset--font-family--pt-serif) !important;}.has-petrona-font-family{font-family: var(--wp--preset--font-family--petrona) !important;}.has-piazzolla-font-family{font-family: var(--wp--preset--font-family--piazzolla) !important;}.has-playfair-display-font-family{font-family: var(--wp--preset--font-family--playfair-display) !important;}.has-plus-jakarta-sans-font-family{font-family: var(--wp--preset--font-family--plus-jakarta-sans) !important;}.has-poppins-font-family{font-family: var(--wp--preset--font-family--poppins) !important;}.has-raleway-font-family{font-family: var(--wp--preset--font-family--raleway) !important;}.has-roboto-font-family{font-family: var(--wp--preset--font-family--roboto) !important;}.has-roboto-slab-font-family{font-family: var(--wp--preset--font-family--roboto-slab) !important;}.has-rubik-font-family{font-family: var(--wp--preset--font-family--rubik) !important;}.has-rufina-font-family{font-family: var(--wp--preset--font-family--rufina) !important;}.has-sora-font-family{font-family: var(--wp--preset--font-family--sora) !important;}.has-source-sans-3-font-family{font-family: var(--wp--preset--font-family--source-sans-3) !important;}.has-source-serif-4-font-family{font-family: var(--wp--preset--font-family--source-serif-4) !important;}.has-space-mono-font-family{font-family: var(--wp--preset--font-family--space-mono) !important;}.has-syne-font-family{font-family: var(--wp--preset--font-family--syne) !important;}.has-texturina-font-family{font-family: var(--wp--preset--font-family--texturina) !important;}.has-urbanist-font-family{font-family: var(--wp--preset--font-family--urbanist) !important;}.has-work-sans-font-family{font-family: var(--wp--preset--font-family--work-sans) !important;}
:where(.wp-block-columns.is-layout-flex){gap: 2em;}:where(.wp-block-columns.is-layout-grid){gap: 2em;}
.wp-block-pullquote{font-size: 1.5em;line-height: 1.6;}
.wp-block-navigation a:where(:not(.wp-element-button)){color: inherit;}
:where(.wp-block-post-template.is-layout-flex){gap: 1.25em;}:where(.wp-block-post-template.is-layout-grid){gap: 1.25em;}
/*
Welcome to Custom CSS!

To learn how this works, see https://wp.me/PEmnE-Bt
*/
body {
	background-color: #fff !important;
}

@media screen and (min-width: 44.375em) {
	body #infinite-footer .container {
		width: 100% !important;
		width: 100% !important;
	}
}

body:not(.custom-background-image) #infinite-footer {
	bottom: 0 !important;
}

body:not(.custom-background-image):before, body:not(.custom-background-image):after {
	height: 0 !important;
}

#page .site-inner header.site-header {
	padding: 0 4.5455% !important;
}

@media screen and (min-width: 61.5625em) {
	#page .site-inner header.site-header {
		padding: 0 4.5455% !important;
	}
}

// css by manish
.site-inner {
	margin: 0 auto;
	position: relative;
	width: 100% !important;
	max-width: 100% !important;
}

.entry-title a, .entry-title {
	font-size: 40px !important;
}

.site {
	margin: 0 !important;
}

.site-header {
	background: #ffffff !important;
}

.site-content {
	margin-top: 50px !important;
}

/* manish */
#page .site-inner header.site-header {
	padding: 0 !important;
}

.header-image a img {
	width: 100% !important;
}

/*  code pankaj */
.modal {
	position: fixed;
	top: 50%;
	left: 50%;
	transform: translate(-50%, calc(-50% - 10px));
	display: flex;
	min-width: 550px;
	font-family: 'Roboto', sans-serif;
	background-color: #fefefe;
	border-radius: 12px;
	box-shadow: 0 5px 26px -8px rgba(0, 0, 0, 0.3);
	z-index: 20;
	transition: all .4s ease;
	opacity: 0;
	pointer-events: none;
}

.modal.active {
	opacity: 1;
	pointer-events: auto;
	transform: translate(-50%, -50%);
}

.modal__overlay {
	position: fixed;
	top: 0;
	left: 0;
	bottom: 0;
	right: 0;
	width: 100%;
	height: 100%;
	background: rgba(0, 0, 0, 0.6);
	pointer-events: none;
	transition: all .4s ease;
	opacity: 0;
}

.modal__overlay.active {
	opacity: 1;
}

.modal__close-btn {
	position: absolute;
	top: 20px;
	right: 20px;
	font-size: 20px;
	cursor: pointer;
	padding: 4px;
}

.modal__left {
	text-align: center;
	font-size: 24px;
	text-transform: uppercase;
	background-color: #242424;
	color: #fefefe;
	border-radius: 12px;
	box-shadow: 17px 0 17px -8px rgba(0, 0, 0, 0.3);
	padding: 26px 20px;
}

.modal__left span {
	display: block;
	font-size: 36px;
}

.modal__discount {
	font-size: 14px;
	font-weight: 300;
	letter-spacing: 3px;
	color: #ebff00;
	margin-top: 32px;
}

.modal__discount span {
	font-size: 60px;
}

.modal__right {
	display: flex;
	flex-direction: column;
	justify-content: center;
	padding: 32px;
}

.modal__text {
	font-size: 24px;
	font-weight: 700;
	text-transform: uppercase;
	margin-bottom: 22px;
}

.modal__info {
	color: #222;
	margin-bottom: 12px;
}

.modal__button {
	align-self: flex-start;
	font-size: 18px;
	font-weight: 700;
	text-transform: uppercase;
	text-decoration: none;
	background-color: #ff5555;
	border-radius: 12px;
	padding: 10px 32px;
	cursor: pointer;
}
</style>
<link rel='stylesheet' id='dashicons-css' href='https://c0.wp.com/c/6.5.5/wp-includes/css/dashicons.min.css' media='all' />
<link rel='stylesheet' id='wp-components-css' href='https://opstree.com/blog/wp-content/plugins/gutenberg/build/components/style.css?ver=17.9.0' media='all' />
<link rel='stylesheet' id='godaddy-styles-css' href='https://opstree.com/blog/wp-content/plugins/coblocks/includes/Dependencies/GoDaddy/Styles/build/latest.css?ver=2.0.2' media='all' />
<!-- `jetpack_likes` is included in the concatenated jetpack.css -->
<link rel='stylesheet' id='twentysixteen-fonts-css' href='https://opstree.com/blog/wp-content/themes/twentysixteen/fonts/merriweather-plus-montserrat-plus-inconsolata.css?ver=20230328' media='all' />
<link rel='stylesheet' id='genericons-css' href='https://c0.wp.com/p/jetpack/13.3.2/_inc/genericons/genericons/genericons.css' media='all' />
<link rel='stylesheet' id='twentysixteen-style-css' href='https://opstree.com/blog/wp-content/themes/twentysixteen/style.css?ver=20240402' media='all' />
<link rel='stylesheet' id='twentysixteen-block-style-css' href='https://opstree.com/blog/wp-content/themes/twentysixteen/css/blocks.css?ver=20240117' media='all' />
<!--[if lt IE 10]>
<link rel='stylesheet' id='twentysixteen-ie-css' href='https://opstree.com/blog/wp-content/themes/twentysixteen/css/ie.css?ver=20170530' media='all' />
<![endif]-->
<!--[if lt IE 9]>
<link rel='stylesheet' id='twentysixteen-ie8-css' href='https://opstree.com/blog/wp-content/themes/twentysixteen/css/ie8.css?ver=20170530' media='all' />
<![endif]-->
<!--[if lt IE 8]>
<link rel='stylesheet' id='twentysixteen-ie7-css' href='https://opstree.com/blog/wp-content/themes/twentysixteen/css/ie7.css?ver=20170530' media='all' />
<![endif]-->
<!-- `jetpack-authors-widget` is included in the concatenated jetpack.css -->
<!-- `sharedaddy` is included in the concatenated jetpack.css -->
<link rel='stylesheet' id='social-logos-css' href='https://c0.wp.com/p/jetpack/13.3.2/_inc/social-logos/social-logos.min.css' media='all' />
<link rel='stylesheet' id='jetpack_css-css' href='https://c0.wp.com/p/jetpack/13.3.2/css/jetpack.css' media='all' />
<script id="jetpack_related-posts-js-extra">
var related_posts_js_options = {"post_heading":"h4"};
</script>
<script src="https://c0.wp.com/p/jetpack/13.3.2/_inc/build/related-posts/related-posts.min.js" id="jetpack_related-posts-js"></script>
<!--[if lt IE 9]>
<script src="https://opstree.com/blog/wp-content/themes/twentysixteen/js/html5.js?ver=3.7.3" id="twentysixteen-html5-js"></script>
<![endif]-->
<script src="https://c0.wp.com/c/6.5.5/wp-includes/js/jquery/jquery.min.js" id="jquery-core-js"></script>
<script src="https://c0.wp.com/c/6.5.5/wp-includes/js/jquery/jquery-migrate.min.js" id="jquery-migrate-js"></script>
<script id="twentysixteen-script-js-extra">
var screenReaderText = {"expand":"expand child menu","collapse":"collapse child menu"};
</script>
<script src="https://opstree.com/blog/wp-content/themes/twentysixteen/js/functions.js?ver=20230629" id="twentysixteen-script-js" defer data-wp-strategy="defer"></script>

<!-- Google tag (gtag.js) snippet added by Site Kit -->

<!-- Google Analytics snippet added by Site Kit -->
<script src="https://www.googletagmanager.com/gtag/js?id=GT-TXHNS2X" id="google_gtagjs-js" async></script>
<script id="google_gtagjs-js-after">
window.dataLayer = window.dataLayer || [];function gtag(){dataLayer.push(arguments);}
gtag("set","linker",{"domains":["opstree.com"]});
gtag("js", new Date());
gtag("set", "developer_id.dZTNiMT", true);
gtag("config", "GT-TXHNS2X");
</script>

<!-- End Google tag (gtag.js) snippet added by Site Kit -->
<link rel="https://api.w.org/" href="https://opstree.com/blog/wp-json/" /><link rel="alternate" type="application/json" href="https://opstree.com/blog/wp-json/wp/v2/posts/12726" /><link rel="EditURI" type="application/rsd+xml" title="RSD" href="https://opstree.com/blog/xmlrpc.php?rsd" />
<meta name="generator" content="WordPress 6.5.5" />
<link rel='shortlink' href='https://wp.me/pfDBOm-3jg' />
<link rel="alternate" type="application/json+oembed" href="https://opstree.com/blog/wp-json/oembed/1.0/embed?url=https%3A%2F%2Fopstree.com%2Fblog%2F2023%2F01%2F24%2Fprotected-efk-stack-setup-for-kubernetes%2F" />
<link rel="alternate" type="text/xml+oembed" href="https://opstree.com/blog/wp-json/oembed/1.0/embed?url=https%3A%2F%2Fopstree.com%2Fblog%2F2023%2F01%2F24%2Fprotected-efk-stack-setup-for-kubernetes%2F&#038;format=xml" />
<meta name="generator" content="Site Kit by Google 1.126.0" />	<style>img#wpstats{display:none}</style>
		<meta name="google-site-verification" content="U9dV6Sao2H5zCTsZKjzJsdb88b0HhQi-h8cPYyGxrpo">		<style type="text/css" id="twentysixteen-header-css">
		.site-branding {
			margin: 0 auto 0 0;
		}

		.site-branding .site-title,
		.site-description {
			clip: rect(1px, 1px, 1px, 1px);
			position: absolute;
		}
		</style>
		<link rel="icon" href="https://i0.wp.com/opstree.com/blog/wp-content/uploads/2024/12/cropped-FavIcon-OpsTree.png?fit=32%2C32&#038;ssl=1" sizes="32x32" />
<link rel="icon" href="https://i0.wp.com/opstree.com/blog/wp-content/uploads/2024/12/cropped-FavIcon-OpsTree.png?fit=192%2C192&#038;ssl=1" sizes="192x192" />
<link rel="apple-touch-icon" href="https://i0.wp.com/opstree.com/blog/wp-content/uploads/2024/12/cropped-FavIcon-OpsTree.png?fit=180%2C180&#038;ssl=1" />
<meta name="msapplication-TileImage" content="https://i0.wp.com/opstree.com/blog/wp-content/uploads/2024/12/cropped-FavIcon-OpsTree.png?fit=270%2C270&#038;ssl=1" />
<link rel="stylesheet" type="text/css" id="wp-custom-css" href="https://opstree.com/blog/?custom-css=330b11a510" /></head>

<body class="post-template-default single single-post postid-12726 single-format-standard wp-custom-logo wp-embed-responsive is-twentysixteen group-blog">
<div id="page" class="site">
	<div class="site-inner">
		<a class="skip-link screen-reader-text" href="#content">
			Skip to content		</a>

		<header id="masthead" class="site-header">
			<div class="site-header-main">
				<div class="site-branding">
					<a href="https://opstree.com/blog/" class="custom-logo-link" rel="home"><img width="203" height="41" src="https://i0.wp.com/opstree.com/blog/wp-content/uploads/2020/08/logo-black.png?fit=203%2C41&amp;ssl=1" class="custom-logo" alt="Opstree" decoding="async" data-attachment-id="3985" data-permalink="https://opstree.com/blog/logo-black/" data-orig-file="https://i0.wp.com/opstree.com/blog/wp-content/uploads/2020/08/logo-black.png?fit=203%2C41&amp;ssl=1" data-orig-size="203,41" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="logo-black" data-image-description="" data-image-caption="" data-medium-file="https://i0.wp.com/opstree.com/blog/wp-content/uploads/2020/08/logo-black.png?fit=203%2C41&amp;ssl=1" data-large-file="https://i0.wp.com/opstree.com/blog/wp-content/uploads/2020/08/logo-black.png?fit=203%2C41&amp;ssl=1" /></a>
											<p class="site-title"><a href="https://opstree.com/blog/" rel="home">DEVOPS DONE RIGHT.</a></p>
												<p class="site-description">A blog site on our Real life experiences with various phases of DevOps starting from VCS, Build &amp; Release, CI/CD, Cloud, Monitoring, Containerization.</p>
									</div><!-- .site-branding -->

									<button id="menu-toggle" class="menu-toggle">Menu</button>

					<div id="site-header-menu" class="site-header-menu">
													<nav id="site-navigation" class="main-navigation" aria-label="Primary Menu">
								<div class="menu-primary-container"><ul id="menu-primary" class="primary-menu"><li id="menu-item-6" class="menu-item menu-item-type-custom menu-item-object-custom menu-item-6"><a href="/">Home</a></li>
<li id="menu-item-13367" class="menu-item menu-item-type-custom menu-item-object-custom menu-item-13367"><a target="_blank" rel="noopener" href="https://opstree.com/services/">Services</a></li>
<li id="menu-item-13370" class="menu-item menu-item-type-custom menu-item-object-custom menu-item-13370"><a target="_blank" rel="noopener" href="https://opstree.com/case-study/">Case Study</a></li>
<li id="menu-item-7" class="menu-item menu-item-type-post_type menu-item-object-page menu-item-7"><a href="https://opstree.com/blog/contact/">Contact</a></li>
<li id="menu-item-7661" class="menu-item menu-item-type-custom menu-item-object-custom menu-item-7661"><a href="/blog/write-for-us">Sign Up</a></li>
</ul></div>							</nav><!-- .main-navigation -->
						
											</div><!-- .site-header-menu -->
							</div><!-- .site-header-main -->

					</header><!-- .site-header -->

		<div id="content" class="site-content">

<div id="primary" class="content-area">
	<main id="main" class="site-main">
		
<article id="post-12726" class="post-12726 post type-post status-publish format-standard hentry category-devops tag-automation tag-devops tag-devops-solutioning tag-efk tag-kubernetes tag-monitoring tag-technical-blogs">
	<header class="entry-header">
		<h1 class="entry-title">Protected EFK Stack Setup for Kubernetes</h1>	</header><!-- .entry-header -->

	
	
	<div class="entry-content">
		
<hr class="wp-block-separator has-alpha-channel-opacity" />



<p class="has-text-align-justify">In this blog, we will see how we can deploy the Elasticsearch, Fluent-bit, and Kibana (EFK) stack on Kubernetes. EFK stack’s prime objective is to reliably and securely retrieve data from the K8s cluster in any format, as well as to facilitate anytime searching, analyzing, and visualizing of the data.</p>



<figure class="wp-block-image"><img decoding="async" src="https://i0.wp.com/opstree.com/blog//wp-content/uploads/2023/01/fd618-1j4l3cowyx0orfe0qqiodbq.png?w=840&#038;ssl=1" alt="" data-recalc-dims="1" /></figure>



<h3 class="wp-block-heading">What is EFK&nbsp;Stack?</h3>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="has-medium-gray-color has-text-color has-medium-font-size">EFK stands for <strong>Elasticsearch, Fluent bit, and Kibana</strong>.</p>
</blockquote>



<p class="has-text-align-justify"><strong>Elasticsearch</strong> is a scalable and distributed search engine that is commonly used to store large amounts of log data. It is a NoSQL database. Its primary function is to store and retrieve logs from fluent bit.</p>



<p class="has-text-align-justify"><strong>Fluent Bit</strong> is a logging and metrics processor and forwarder that is extremely fast, lightweight, and highly scalable. Because of its performance-oriented design, it is simple to collect events from various sources and ship them to various destinations without complexity.</p>



<span id="more-12726"></span>



<p class="has-text-align-justify"><strong>Kibana </strong>is a graphical user interface (GUI) tool for data visualization, querying, and dashboards. It is a query engine that lets you explore your log data through a web interface, create visualizations for event logs, and filter data to detect problems. Kibana is being used to query elasticsearch indexed data.</p>



<h3 class="wp-block-heading">Why do we need EFK&nbsp;Stack?</h3>



<ul>
<li>Using the EFK stack in your Kubernetes cluster can make it much easier to collect, store, and analyze log data from all the pods and nodes in your cluster, making it more manageable and more accessible for different users.</li>



<li>The kubectl logs command is useful for looking at logs from individual pods, but it can quickly become unwieldy when you have a large number of pods running in your cluster. </li>



<li>With the EFK stack, you can collect logs from all the nodes and pods in your cluster and store them in a central location. It allows you to quickly troubleshoot issues and identify patterns in your log data. </li>



<li>It also enables people who are not familiar with using the command line to check logs and keep track of the Kubernetes cluster and the applications that are deployed on it. </li>



<li>It also allows you to easily create alerts, dashboards, and create monitoring and reporting capabilities that can give you an overview of your system’s health and performance, and It will notify you in real-time if something goes wrong. </li>
</ul>



<figure class="wp-block-image"><img decoding="async" src="https://i0.wp.com/opstree.com/blog//wp-content/uploads/2023/01/fbb0c-1cx_q-t0x82ghsfq-fvuvcw.png?w=840&#038;ssl=1" alt="" data-recalc-dims="1" /></figure>



<h6 class="wp-block-heading">In this tutorial, we will be deploying EFK components as&nbsp;follows:</h6>



<ol>
<li><strong>Elasticsearch</strong> is deployed as statefulset as it stores the log data.</li>



<li><strong>Kibana</strong> is deployed as deployment and connects to elasticsearch service endpoint.</li>



<li><strong>Fluent-bit</strong> is deployed as a daemonset to gather the container logs from every node. It connects to the Elasticsearch service endpoint to forward the logs.</li>
</ol>



<h6 class="wp-block-heading has-medium-gray-color has-text-color has-medium-font-size"><strong>So let’s get started with EFK stack deployment.</strong></h6>



<h3 class="wp-block-heading">Creating a&nbsp;Namespace</h3>



<p class="has-text-align-justify">It’s a good practice to create a separate namespace for every functional unit in Kubernetes. By creating a namespace for each functional unit, you can easily see which resources belong to which unit and manage them accordingly.</p>



<p class="has-text-align-justify">let&#8217;s create a namespace</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120368818" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-namespace-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="namespace.yaml">
        <tr>
          <td id="file-namespace-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-namespace-yaml-LC1" class="blob-code blob-code-inner js-file-line">kind: Namespace</td>
        </tr>
        <tr>
          <td id="file-namespace-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-namespace-yaml-LC2" class="blob-code blob-code-inner js-file-line">apiVersion: v1</td>
        </tr>
        <tr>
          <td id="file-namespace-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-namespace-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-namespace-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-namespace-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: kube-logging</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/b09f6c3fb76947ac211bff561a1784db/raw/e04615d0e02b6d1ac2f82a46fadc8d8ac4c653d3/namespace.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/b09f6c3fb76947ac211bff561a1784db#file-namespace-yaml" class="Link--inTextBlock">
          namespace.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<p>Let’s apply the manifest file created above, run the following command:</p>



<pre class="wp-block-preformatted has-white-color has-dark-gray-background-color has-text-color has-background">$ Kubectl apply -f namespace.yaml</pre>



<h3 class="wp-block-heading">Creating a&nbsp;Secret</h3>



<p class="has-text-align-justify">Secrets in Kubernetes (K8s) are native sources for storing and managing sensitive data such as passwords, cloud access keys, or authentication tokens. You must distribute this information across your Kubernetes clusters while also protecting it.</p>



<p class="has-text-align-justify">Let&#8217;s create a secret for an elasticsearch password to make Kibana logging protected.</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120370768" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-secrets-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="secrets.yaml">
        <tr>
          <td id="file-secrets-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-secrets-yaml-LC1" class="blob-code blob-code-inner js-file-line">apiVersion: v1</td>
        </tr>
        <tr>
          <td id="file-secrets-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-secrets-yaml-LC2" class="blob-code blob-code-inner js-file-line">kind: Secret</td>
        </tr>
        <tr>
          <td id="file-secrets-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-secrets-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-secrets-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-secrets-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: kibana-password</td>
        </tr>
        <tr>
          <td id="file-secrets-yaml-L5" class="blob-num js-line-number js-blob-rnum" data-line-number="5"></td>
          <td id="file-secrets-yaml-LC5" class="blob-code blob-code-inner js-file-line">  namespace: kube-logging</td>
        </tr>
        <tr>
          <td id="file-secrets-yaml-L6" class="blob-num js-line-number js-blob-rnum" data-line-number="6"></td>
          <td id="file-secrets-yaml-LC6" class="blob-code blob-code-inner js-file-line">type: Opaque</td>
        </tr>
        <tr>
          <td id="file-secrets-yaml-L7" class="blob-num js-line-number js-blob-rnum" data-line-number="7"></td>
          <td id="file-secrets-yaml-LC7" class="blob-code blob-code-inner js-file-line">data:</td>
        </tr>
        <tr>
          <td id="file-secrets-yaml-L8" class="blob-num js-line-number js-blob-rnum" data-line-number="8"></td>
          <td id="file-secrets-yaml-LC8" class="blob-code blob-code-inner js-file-line">  password: cGFzc3dvcmQ=</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/e51423132fc25dedbc83c0449aa65306/raw/107e986fc237833998867d3e2ff11229632e16c0/secrets.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/e51423132fc25dedbc83c0449aa65306#file-secrets-yaml" class="Link--inTextBlock">
          secrets.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<p>Let’s apply the manifest file created above, run the following command:</p>



<pre class="wp-block-preformatted has-white-color has-dark-gray-background-color has-text-color has-background">$ Kubectl apply -f secrets.yaml</pre>



<h3 class="wp-block-heading">Deploy Elasticsearch as Statefulset</h3>



<p class="has-text-align-justify">Deploying Elasticsearch as a StatefulSets provides stable unique network identities and stable storage. Elasticsearch stores a large amount of data and it’s important that data stored in Elasticsearch should be persisted and available even if the pod is deleted or recreated.</p>



<h5 class="wp-block-heading has-text-align-justify">Creating the Headless&nbsp;Service</h5>



<p class="has-text-align-justify">Now Let&#8217;s set up elasticsearch, a Kubernetes headless service that will define a DNS domain for pods. A headless service lacks load balancing and has no static IP address.</p>



<p> Let&#8217;s create a Headless&nbsp;Service for an elasticsearch.</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120370808" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-es-svc-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="es-svc.yaml">
        <tr>
          <td id="file-es-svc-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-es-svc-yaml-LC1" class="blob-code blob-code-inner js-file-line">kind: Service</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-es-svc-yaml-LC2" class="blob-code blob-code-inner js-file-line">apiVersion: v1</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-es-svc-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-es-svc-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: elasticsearch</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L5" class="blob-num js-line-number js-blob-rnum" data-line-number="5"></td>
          <td id="file-es-svc-yaml-LC5" class="blob-code blob-code-inner js-file-line">  namespace: kube-logging</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L6" class="blob-num js-line-number js-blob-rnum" data-line-number="6"></td>
          <td id="file-es-svc-yaml-LC6" class="blob-code blob-code-inner js-file-line">  labels:</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L7" class="blob-num js-line-number js-blob-rnum" data-line-number="7"></td>
          <td id="file-es-svc-yaml-LC7" class="blob-code blob-code-inner js-file-line">    app: elasticsearch</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L8" class="blob-num js-line-number js-blob-rnum" data-line-number="8"></td>
          <td id="file-es-svc-yaml-LC8" class="blob-code blob-code-inner js-file-line">spec:</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L9" class="blob-num js-line-number js-blob-rnum" data-line-number="9"></td>
          <td id="file-es-svc-yaml-LC9" class="blob-code blob-code-inner js-file-line">  selector:</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L10" class="blob-num js-line-number js-blob-rnum" data-line-number="10"></td>
          <td id="file-es-svc-yaml-LC10" class="blob-code blob-code-inner js-file-line">    app: elasticsearch</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L11" class="blob-num js-line-number js-blob-rnum" data-line-number="11"></td>
          <td id="file-es-svc-yaml-LC11" class="blob-code blob-code-inner js-file-line">  clusterIP: None</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L12" class="blob-num js-line-number js-blob-rnum" data-line-number="12"></td>
          <td id="file-es-svc-yaml-LC12" class="blob-code blob-code-inner js-file-line">  ports:</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L13" class="blob-num js-line-number js-blob-rnum" data-line-number="13"></td>
          <td id="file-es-svc-yaml-LC13" class="blob-code blob-code-inner js-file-line">    &#8211; port: 9200</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L14" class="blob-num js-line-number js-blob-rnum" data-line-number="14"></td>
          <td id="file-es-svc-yaml-LC14" class="blob-code blob-code-inner js-file-line">      name: rest</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L15" class="blob-num js-line-number js-blob-rnum" data-line-number="15"></td>
          <td id="file-es-svc-yaml-LC15" class="blob-code blob-code-inner js-file-line">    &#8211; port: 9300</td>
        </tr>
        <tr>
          <td id="file-es-svc-yaml-L16" class="blob-num js-line-number js-blob-rnum" data-line-number="16"></td>
          <td id="file-es-svc-yaml-LC16" class="blob-code blob-code-inner js-file-line">      name: inter-node</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/15b1b7cc9d22c52bb6ea7ef82f02dd33/raw/5610ec30205ec1277d22f4fec8b11f2ebd0f3f6f/es-svc.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/15b1b7cc9d22c52bb6ea7ef82f02dd33#file-es-svc-yaml" class="Link--inTextBlock">
          es-svc.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<p>Let’s apply the elasticsearch service file created above, run the following command:</p>



<pre class="wp-block-preformatted has-white-color has-dark-gray-background-color has-text-color has-background">$ Kubectl apply -f es-svs.yaml</pre>



<h5 class="wp-block-heading">Creating the Elasticsearch StatefulSet</h5>



<p class="has-text-align-justify">Deploying Elasticsearch as a StatefulSets pods are created and deleted in a specific order, ensuring that your data is not lost. This is especially useful for Elasticsearch, as it helps ensure that data is not lost during deployments and scaling events.</p>



<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
<p class="has-text-align-justify has-small-font-size">When you create a StatefulSet with a Persistent Volume Claim (PVC) template, the default storage class will be used if no custom storage class is specified. To use a custom storage class for the PVCs created by a StatefulSet, you can specify the storage class in the volumeClaimTemplates section of the StatefulSet definition.</p>
</blockquote>



<p> Let&#8217;s create a StatefulSets for an elasticsearch.</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120370841" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-es-sts-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="es-sts.yaml">
        <tr>
          <td id="file-es-sts-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-es-sts-yaml-LC1" class="blob-code blob-code-inner js-file-line">apiVersion: apps/v1</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-es-sts-yaml-LC2" class="blob-code blob-code-inner js-file-line">kind: StatefulSet</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-es-sts-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-es-sts-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: es-cluster</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L5" class="blob-num js-line-number js-blob-rnum" data-line-number="5"></td>
          <td id="file-es-sts-yaml-LC5" class="blob-code blob-code-inner js-file-line">  namespace: kube-logging</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L6" class="blob-num js-line-number js-blob-rnum" data-line-number="6"></td>
          <td id="file-es-sts-yaml-LC6" class="blob-code blob-code-inner js-file-line">spec:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L7" class="blob-num js-line-number js-blob-rnum" data-line-number="7"></td>
          <td id="file-es-sts-yaml-LC7" class="blob-code blob-code-inner js-file-line">  serviceName: elasticsearch</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L8" class="blob-num js-line-number js-blob-rnum" data-line-number="8"></td>
          <td id="file-es-sts-yaml-LC8" class="blob-code blob-code-inner js-file-line">  replicas: 3</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L9" class="blob-num js-line-number js-blob-rnum" data-line-number="9"></td>
          <td id="file-es-sts-yaml-LC9" class="blob-code blob-code-inner js-file-line">  selector:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L10" class="blob-num js-line-number js-blob-rnum" data-line-number="10"></td>
          <td id="file-es-sts-yaml-LC10" class="blob-code blob-code-inner js-file-line">    matchLabels:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L11" class="blob-num js-line-number js-blob-rnum" data-line-number="11"></td>
          <td id="file-es-sts-yaml-LC11" class="blob-code blob-code-inner js-file-line">      app: elasticsearch</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L12" class="blob-num js-line-number js-blob-rnum" data-line-number="12"></td>
          <td id="file-es-sts-yaml-LC12" class="blob-code blob-code-inner js-file-line">  template:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L13" class="blob-num js-line-number js-blob-rnum" data-line-number="13"></td>
          <td id="file-es-sts-yaml-LC13" class="blob-code blob-code-inner js-file-line">    metadata:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L14" class="blob-num js-line-number js-blob-rnum" data-line-number="14"></td>
          <td id="file-es-sts-yaml-LC14" class="blob-code blob-code-inner js-file-line">      labels:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L15" class="blob-num js-line-number js-blob-rnum" data-line-number="15"></td>
          <td id="file-es-sts-yaml-LC15" class="blob-code blob-code-inner js-file-line">        app: elasticsearch</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L16" class="blob-num js-line-number js-blob-rnum" data-line-number="16"></td>
          <td id="file-es-sts-yaml-LC16" class="blob-code blob-code-inner js-file-line">    spec:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L17" class="blob-num js-line-number js-blob-rnum" data-line-number="17"></td>
          <td id="file-es-sts-yaml-LC17" class="blob-code blob-code-inner js-file-line">      containers:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L18" class="blob-num js-line-number js-blob-rnum" data-line-number="18"></td>
          <td id="file-es-sts-yaml-LC18" class="blob-code blob-code-inner js-file-line">      &#8211; name: elasticsearch</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L19" class="blob-num js-line-number js-blob-rnum" data-line-number="19"></td>
          <td id="file-es-sts-yaml-LC19" class="blob-code blob-code-inner js-file-line">        image: docker.elastic.co/elasticsearch/elasticsearch:7.2.0</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L20" class="blob-num js-line-number js-blob-rnum" data-line-number="20"></td>
          <td id="file-es-sts-yaml-LC20" class="blob-code blob-code-inner js-file-line">        resources:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L21" class="blob-num js-line-number js-blob-rnum" data-line-number="21"></td>
          <td id="file-es-sts-yaml-LC21" class="blob-code blob-code-inner js-file-line">            limits:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L22" class="blob-num js-line-number js-blob-rnum" data-line-number="22"></td>
          <td id="file-es-sts-yaml-LC22" class="blob-code blob-code-inner js-file-line">              cpu: 1000m</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L23" class="blob-num js-line-number js-blob-rnum" data-line-number="23"></td>
          <td id="file-es-sts-yaml-LC23" class="blob-code blob-code-inner js-file-line">              memory: 2Gi</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L24" class="blob-num js-line-number js-blob-rnum" data-line-number="24"></td>
          <td id="file-es-sts-yaml-LC24" class="blob-code blob-code-inner js-file-line">            requests:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L25" class="blob-num js-line-number js-blob-rnum" data-line-number="25"></td>
          <td id="file-es-sts-yaml-LC25" class="blob-code blob-code-inner js-file-line">              cpu: 500m</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L26" class="blob-num js-line-number js-blob-rnum" data-line-number="26"></td>
          <td id="file-es-sts-yaml-LC26" class="blob-code blob-code-inner js-file-line">              memory: 1Gi</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L27" class="blob-num js-line-number js-blob-rnum" data-line-number="27"></td>
          <td id="file-es-sts-yaml-LC27" class="blob-code blob-code-inner js-file-line">        ports:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L28" class="blob-num js-line-number js-blob-rnum" data-line-number="28"></td>
          <td id="file-es-sts-yaml-LC28" class="blob-code blob-code-inner js-file-line">        &#8211; containerPort: 9200</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L29" class="blob-num js-line-number js-blob-rnum" data-line-number="29"></td>
          <td id="file-es-sts-yaml-LC29" class="blob-code blob-code-inner js-file-line">          name: rest</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L30" class="blob-num js-line-number js-blob-rnum" data-line-number="30"></td>
          <td id="file-es-sts-yaml-LC30" class="blob-code blob-code-inner js-file-line">          protocol: TCP</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L31" class="blob-num js-line-number js-blob-rnum" data-line-number="31"></td>
          <td id="file-es-sts-yaml-LC31" class="blob-code blob-code-inner js-file-line">        &#8211; containerPort: 9300</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L32" class="blob-num js-line-number js-blob-rnum" data-line-number="32"></td>
          <td id="file-es-sts-yaml-LC32" class="blob-code blob-code-inner js-file-line">          name: inter-node</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L33" class="blob-num js-line-number js-blob-rnum" data-line-number="33"></td>
          <td id="file-es-sts-yaml-LC33" class="blob-code blob-code-inner js-file-line">          protocol: TCP</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L34" class="blob-num js-line-number js-blob-rnum" data-line-number="34"></td>
          <td id="file-es-sts-yaml-LC34" class="blob-code blob-code-inner js-file-line">        volumeMounts:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L35" class="blob-num js-line-number js-blob-rnum" data-line-number="35"></td>
          <td id="file-es-sts-yaml-LC35" class="blob-code blob-code-inner js-file-line">        &#8211; name: data</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L36" class="blob-num js-line-number js-blob-rnum" data-line-number="36"></td>
          <td id="file-es-sts-yaml-LC36" class="blob-code blob-code-inner js-file-line">          mountPath: /usr/share/elasticsearch/data</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L37" class="blob-num js-line-number js-blob-rnum" data-line-number="37"></td>
          <td id="file-es-sts-yaml-LC37" class="blob-code blob-code-inner js-file-line">        env:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L38" class="blob-num js-line-number js-blob-rnum" data-line-number="38"></td>
          <td id="file-es-sts-yaml-LC38" class="blob-code blob-code-inner js-file-line">          &#8211; name: cluster.name</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L39" class="blob-num js-line-number js-blob-rnum" data-line-number="39"></td>
          <td id="file-es-sts-yaml-LC39" class="blob-code blob-code-inner js-file-line">            value: k8s-logs</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L40" class="blob-num js-line-number js-blob-rnum" data-line-number="40"></td>
          <td id="file-es-sts-yaml-LC40" class="blob-code blob-code-inner js-file-line">          &#8211; name: node.name</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L41" class="blob-num js-line-number js-blob-rnum" data-line-number="41"></td>
          <td id="file-es-sts-yaml-LC41" class="blob-code blob-code-inner js-file-line">            valueFrom:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L42" class="blob-num js-line-number js-blob-rnum" data-line-number="42"></td>
          <td id="file-es-sts-yaml-LC42" class="blob-code blob-code-inner js-file-line">              fieldRef:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L43" class="blob-num js-line-number js-blob-rnum" data-line-number="43"></td>
          <td id="file-es-sts-yaml-LC43" class="blob-code blob-code-inner js-file-line">                fieldPath: metadata.name</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L44" class="blob-num js-line-number js-blob-rnum" data-line-number="44"></td>
          <td id="file-es-sts-yaml-LC44" class="blob-code blob-code-inner js-file-line">          &#8211; name: discovery.seed_hosts</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L45" class="blob-num js-line-number js-blob-rnum" data-line-number="45"></td>
          <td id="file-es-sts-yaml-LC45" class="blob-code blob-code-inner js-file-line">            value: &quot;es-cluster-0.elasticsearch,es-cluster-1.elasticsearch,es-cluster-2.elasticsearch&quot;</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L46" class="blob-num js-line-number js-blob-rnum" data-line-number="46"></td>
          <td id="file-es-sts-yaml-LC46" class="blob-code blob-code-inner js-file-line">          &#8211; name: cluster.initial_master_nodes</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L47" class="blob-num js-line-number js-blob-rnum" data-line-number="47"></td>
          <td id="file-es-sts-yaml-LC47" class="blob-code blob-code-inner js-file-line">            value: &quot;es-cluster-0,es-cluster-1,es-cluster-2&quot;</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L48" class="blob-num js-line-number js-blob-rnum" data-line-number="48"></td>
          <td id="file-es-sts-yaml-LC48" class="blob-code blob-code-inner js-file-line">          &#8211; name: network.host</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L49" class="blob-num js-line-number js-blob-rnum" data-line-number="49"></td>
          <td id="file-es-sts-yaml-LC49" class="blob-code blob-code-inner js-file-line">            value: &quot;0.0.0.0&quot;</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L50" class="blob-num js-line-number js-blob-rnum" data-line-number="50"></td>
          <td id="file-es-sts-yaml-LC50" class="blob-code blob-code-inner js-file-line">          &#8211; name: xpack.security.enabled</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L51" class="blob-num js-line-number js-blob-rnum" data-line-number="51"></td>
          <td id="file-es-sts-yaml-LC51" class="blob-code blob-code-inner js-file-line">            value: &quot;true&quot;</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L52" class="blob-num js-line-number js-blob-rnum" data-line-number="52"></td>
          <td id="file-es-sts-yaml-LC52" class="blob-code blob-code-inner js-file-line">          &#8211; name: xpack.monitoring.collection.enabled</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L53" class="blob-num js-line-number js-blob-rnum" data-line-number="53"></td>
          <td id="file-es-sts-yaml-LC53" class="blob-code blob-code-inner js-file-line">            value: &quot;true&quot;   </td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L54" class="blob-num js-line-number js-blob-rnum" data-line-number="54"></td>
          <td id="file-es-sts-yaml-LC54" class="blob-code blob-code-inner js-file-line">          &#8211; name: ES_JAVA_OPTS</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L55" class="blob-num js-line-number js-blob-rnum" data-line-number="55"></td>
          <td id="file-es-sts-yaml-LC55" class="blob-code blob-code-inner js-file-line">            value: &quot;-Xms512m -Xmx512m&quot;   </td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L56" class="blob-num js-line-number js-blob-rnum" data-line-number="56"></td>
          <td id="file-es-sts-yaml-LC56" class="blob-code blob-code-inner js-file-line">          &#8211; name: ELASTIC_PASSWORD</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L57" class="blob-num js-line-number js-blob-rnum" data-line-number="57"></td>
          <td id="file-es-sts-yaml-LC57" class="blob-code blob-code-inner js-file-line">            valueFrom:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L58" class="blob-num js-line-number js-blob-rnum" data-line-number="58"></td>
          <td id="file-es-sts-yaml-LC58" class="blob-code blob-code-inner js-file-line">              secretKeyRef:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L59" class="blob-num js-line-number js-blob-rnum" data-line-number="59"></td>
          <td id="file-es-sts-yaml-LC59" class="blob-code blob-code-inner js-file-line">                name: kibana-password</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L60" class="blob-num js-line-number js-blob-rnum" data-line-number="60"></td>
          <td id="file-es-sts-yaml-LC60" class="blob-code blob-code-inner js-file-line">                key: password     </td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L61" class="blob-num js-line-number js-blob-rnum" data-line-number="61"></td>
          <td id="file-es-sts-yaml-LC61" class="blob-code blob-code-inner js-file-line">      initContainers:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L62" class="blob-num js-line-number js-blob-rnum" data-line-number="62"></td>
          <td id="file-es-sts-yaml-LC62" class="blob-code blob-code-inner js-file-line">      &#8211; name: fix-permissions</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L63" class="blob-num js-line-number js-blob-rnum" data-line-number="63"></td>
          <td id="file-es-sts-yaml-LC63" class="blob-code blob-code-inner js-file-line">        image: busybox</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L64" class="blob-num js-line-number js-blob-rnum" data-line-number="64"></td>
          <td id="file-es-sts-yaml-LC64" class="blob-code blob-code-inner js-file-line">        command: [&quot;sh&quot;, &quot;-c&quot;, &quot;chown -R 1000:1000 /usr/share/elasticsearch/data&quot;]</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L65" class="blob-num js-line-number js-blob-rnum" data-line-number="65"></td>
          <td id="file-es-sts-yaml-LC65" class="blob-code blob-code-inner js-file-line">        securityContext:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L66" class="blob-num js-line-number js-blob-rnum" data-line-number="66"></td>
          <td id="file-es-sts-yaml-LC66" class="blob-code blob-code-inner js-file-line">          privileged: true</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L67" class="blob-num js-line-number js-blob-rnum" data-line-number="67"></td>
          <td id="file-es-sts-yaml-LC67" class="blob-code blob-code-inner js-file-line">        volumeMounts:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L68" class="blob-num js-line-number js-blob-rnum" data-line-number="68"></td>
          <td id="file-es-sts-yaml-LC68" class="blob-code blob-code-inner js-file-line">        &#8211; name: data</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L69" class="blob-num js-line-number js-blob-rnum" data-line-number="69"></td>
          <td id="file-es-sts-yaml-LC69" class="blob-code blob-code-inner js-file-line">          mountPath: /usr/share/elasticsearch/data</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L70" class="blob-num js-line-number js-blob-rnum" data-line-number="70"></td>
          <td id="file-es-sts-yaml-LC70" class="blob-code blob-code-inner js-file-line">      &#8211; name: increase-vm-max-map</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L71" class="blob-num js-line-number js-blob-rnum" data-line-number="71"></td>
          <td id="file-es-sts-yaml-LC71" class="blob-code blob-code-inner js-file-line">        image: busybox</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L72" class="blob-num js-line-number js-blob-rnum" data-line-number="72"></td>
          <td id="file-es-sts-yaml-LC72" class="blob-code blob-code-inner js-file-line">        command: [&quot;sysctl&quot;, &quot;-w&quot;, &quot;vm.max_map_count=262144&quot;]</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L73" class="blob-num js-line-number js-blob-rnum" data-line-number="73"></td>
          <td id="file-es-sts-yaml-LC73" class="blob-code blob-code-inner js-file-line">        securityContext:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L74" class="blob-num js-line-number js-blob-rnum" data-line-number="74"></td>
          <td id="file-es-sts-yaml-LC74" class="blob-code blob-code-inner js-file-line">          privileged: true</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L75" class="blob-num js-line-number js-blob-rnum" data-line-number="75"></td>
          <td id="file-es-sts-yaml-LC75" class="blob-code blob-code-inner js-file-line">      &#8211; name: increase-fd-ulimit</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L76" class="blob-num js-line-number js-blob-rnum" data-line-number="76"></td>
          <td id="file-es-sts-yaml-LC76" class="blob-code blob-code-inner js-file-line">        image: busybox</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L77" class="blob-num js-line-number js-blob-rnum" data-line-number="77"></td>
          <td id="file-es-sts-yaml-LC77" class="blob-code blob-code-inner js-file-line">        command: [&quot;sh&quot;, &quot;-c&quot;, &quot;ulimit -n 65536&quot;]</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L78" class="blob-num js-line-number js-blob-rnum" data-line-number="78"></td>
          <td id="file-es-sts-yaml-LC78" class="blob-code blob-code-inner js-file-line">        securityContext:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L79" class="blob-num js-line-number js-blob-rnum" data-line-number="79"></td>
          <td id="file-es-sts-yaml-LC79" class="blob-code blob-code-inner js-file-line">          privileged: true</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L80" class="blob-num js-line-number js-blob-rnum" data-line-number="80"></td>
          <td id="file-es-sts-yaml-LC80" class="blob-code blob-code-inner js-file-line">  volumeClaimTemplates:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L81" class="blob-num js-line-number js-blob-rnum" data-line-number="81"></td>
          <td id="file-es-sts-yaml-LC81" class="blob-code blob-code-inner js-file-line">  &#8211; metadata:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L82" class="blob-num js-line-number js-blob-rnum" data-line-number="82"></td>
          <td id="file-es-sts-yaml-LC82" class="blob-code blob-code-inner js-file-line">      name: data</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L83" class="blob-num js-line-number js-blob-rnum" data-line-number="83"></td>
          <td id="file-es-sts-yaml-LC83" class="blob-code blob-code-inner js-file-line">      labels:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L84" class="blob-num js-line-number js-blob-rnum" data-line-number="84"></td>
          <td id="file-es-sts-yaml-LC84" class="blob-code blob-code-inner js-file-line">        app: elasticsearch</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L85" class="blob-num js-line-number js-blob-rnum" data-line-number="85"></td>
          <td id="file-es-sts-yaml-LC85" class="blob-code blob-code-inner js-file-line">    spec:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L86" class="blob-num js-line-number js-blob-rnum" data-line-number="86"></td>
          <td id="file-es-sts-yaml-LC86" class="blob-code blob-code-inner js-file-line">      accessModes: [ &quot;ReadWriteOnce&quot; ]</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L87" class="blob-num js-line-number js-blob-rnum" data-line-number="87"></td>
          <td id="file-es-sts-yaml-LC87" class="blob-code blob-code-inner js-file-line">#      storageClassName: do-block-storage</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L88" class="blob-num js-line-number js-blob-rnum" data-line-number="88"></td>
          <td id="file-es-sts-yaml-LC88" class="blob-code blob-code-inner js-file-line">      resources:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L89" class="blob-num js-line-number js-blob-rnum" data-line-number="89"></td>
          <td id="file-es-sts-yaml-LC89" class="blob-code blob-code-inner js-file-line">        requests:</td>
        </tr>
        <tr>
          <td id="file-es-sts-yaml-L90" class="blob-num js-line-number js-blob-rnum" data-line-number="90"></td>
          <td id="file-es-sts-yaml-LC90" class="blob-code blob-code-inner js-file-line">          storage: 10Gi</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/e1df38d6e391af22b55a4ad6220b93bc/raw/5beaea0414617329f11876a887c1462d4ce5aa2f/es-sts.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/e1df38d6e391af22b55a4ad6220b93bc#file-es-sts-yaml" class="Link--inTextBlock">
          es-sts.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<p>Let’s apply the elasticsearch statefulset manifest file created above, run the following command:</p>



<pre class="wp-block-preformatted has-white-color has-dark-gray-background-color has-text-color has-background">$ kubectl apply -f es-sts.yaml</pre>



<h5 class="wp-block-heading">Verify Elasticsearch Deployment</h5>



<p>Let&#8217;s check all Elastisearch pods come into the running state.</p>



<pre class="wp-block-preformatted has-white-color has-dark-gray-background-color has-text-color has-background">$ kubectl get pods -n kube-logging</pre>



<p>To check the health of the Elasticsearch cluster.</p>



<pre class="wp-block-preformatted has-white-color has-dark-gray-background-color has-text-color has-background">$ curl http://localhost:9200/_cluster/health/?pretty</pre>



<p>The status of the Elasticsearch cluster will be shown in the output. If all of the steps were performed properly, the status should be ‘green’.</p>



<p>Let’s move on to Kibana now that we have an Elasticsearch cluster up and running.</p>



<h3 class="wp-block-heading">Deploy Kibana as Deployment</h3>



<p class="has-text-align-justify">Deploying Kibana as a Deployment allows you to easily scale the number of replicas up or down to handle changes in load. This is especially useful for handling large amounts of data and dealing with periods of high traffic and helps you to take advantage of Kubernetes features such as automatic self-healing and automatic scaling which can save resources and cost for running Kibana pods.</p>



<h5 class="wp-block-heading">Creating the Kibana&nbsp;Service</h5>



<p class="has-text-align-justify">Let’s make a NodePort service to access the Kibana UI via the node IP address. For demonstration purposes or testing, however, it’s not considered a best practice for actual production use. The Kubernetes ingress with a ClusterIP service is a more secure and way to expose the Kibana UI.</p>



<p>Let’s create a Service for Kibana.</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120370922" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-kibana-svc-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="kibana-svc.yaml">
        <tr>
          <td id="file-kibana-svc-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-kibana-svc-yaml-LC1" class="blob-code blob-code-inner js-file-line">apiVersion: v1</td>
        </tr>
        <tr>
          <td id="file-kibana-svc-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-kibana-svc-yaml-LC2" class="blob-code blob-code-inner js-file-line">kind: Service</td>
        </tr>
        <tr>
          <td id="file-kibana-svc-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-kibana-svc-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-kibana-svc-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-kibana-svc-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: kibana</td>
        </tr>
        <tr>
          <td id="file-kibana-svc-yaml-L5" class="blob-num js-line-number js-blob-rnum" data-line-number="5"></td>
          <td id="file-kibana-svc-yaml-LC5" class="blob-code blob-code-inner js-file-line">  namespace: kube-logging</td>
        </tr>
        <tr>
          <td id="file-kibana-svc-yaml-L6" class="blob-num js-line-number js-blob-rnum" data-line-number="6"></td>
          <td id="file-kibana-svc-yaml-LC6" class="blob-code blob-code-inner js-file-line">  labels:</td>
        </tr>
        <tr>
          <td id="file-kibana-svc-yaml-L7" class="blob-num js-line-number js-blob-rnum" data-line-number="7"></td>
          <td id="file-kibana-svc-yaml-LC7" class="blob-code blob-code-inner js-file-line">    app: kibana</td>
        </tr>
        <tr>
          <td id="file-kibana-svc-yaml-L8" class="blob-num js-line-number js-blob-rnum" data-line-number="8"></td>
          <td id="file-kibana-svc-yaml-LC8" class="blob-code blob-code-inner js-file-line">spec:</td>
        </tr>
        <tr>
          <td id="file-kibana-svc-yaml-L9" class="blob-num js-line-number js-blob-rnum" data-line-number="9"></td>
          <td id="file-kibana-svc-yaml-LC9" class="blob-code blob-code-inner js-file-line">  ports:</td>
        </tr>
        <tr>
          <td id="file-kibana-svc-yaml-L10" class="blob-num js-line-number js-blob-rnum" data-line-number="10"></td>
          <td id="file-kibana-svc-yaml-LC10" class="blob-code blob-code-inner js-file-line">  &#8211; port: 5601</td>
        </tr>
        <tr>
          <td id="file-kibana-svc-yaml-L11" class="blob-num js-line-number js-blob-rnum" data-line-number="11"></td>
          <td id="file-kibana-svc-yaml-LC11" class="blob-code blob-code-inner js-file-line">  selector:</td>
        </tr>
        <tr>
          <td id="file-kibana-svc-yaml-L12" class="blob-num js-line-number js-blob-rnum" data-line-number="12"></td>
          <td id="file-kibana-svc-yaml-LC12" class="blob-code blob-code-inner js-file-line">    app: kibana</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/3618a8e26761823476b6500930e1a301/raw/f8a7cc77452e91f80db3e868433c0543f88fc92a/kibana-svc.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/3618a8e26761823476b6500930e1a301#file-kibana-svc-yaml" class="Link--inTextBlock">
          kibana-svc.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<p>Let’s apply the kibana service file created above, run the following command:</p>



<pre class="wp-block-preformatted has-white-color has-dark-gray-background-color has-text-color has-background">Kubectl apply -f kibana-svc.yaml</pre>



<h5 class="wp-block-heading">Creating the Kibana Deployment</h5>



<p class="has-text-align-justify">Kibana can be set up as a simple Kubernetes deployment. If you look at the Kibana deployment manifest file, you’ll notice that we have an env variable ELASTICSEARCH_URL defined to configure the Elasticsearch cluster endpoint. Kibana communicates to elasticsearch via the endpoint URL.</p>



<p>Let’s create a Deployment for Kibana.</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120370935" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-kibana-deployment-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="kibana-deployment.yaml">
        <tr>
          <td id="file-kibana-deployment-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-kibana-deployment-yaml-LC1" class="blob-code blob-code-inner js-file-line">apiVersion: apps/v1</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-kibana-deployment-yaml-LC2" class="blob-code blob-code-inner js-file-line">kind: Deployment</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-kibana-deployment-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-kibana-deployment-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: kibana</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L5" class="blob-num js-line-number js-blob-rnum" data-line-number="5"></td>
          <td id="file-kibana-deployment-yaml-LC5" class="blob-code blob-code-inner js-file-line">  namespace: kube-logging</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L6" class="blob-num js-line-number js-blob-rnum" data-line-number="6"></td>
          <td id="file-kibana-deployment-yaml-LC6" class="blob-code blob-code-inner js-file-line">  labels:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L7" class="blob-num js-line-number js-blob-rnum" data-line-number="7"></td>
          <td id="file-kibana-deployment-yaml-LC7" class="blob-code blob-code-inner js-file-line">    app: kibana</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L8" class="blob-num js-line-number js-blob-rnum" data-line-number="8"></td>
          <td id="file-kibana-deployment-yaml-LC8" class="blob-code blob-code-inner js-file-line">spec:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L9" class="blob-num js-line-number js-blob-rnum" data-line-number="9"></td>
          <td id="file-kibana-deployment-yaml-LC9" class="blob-code blob-code-inner js-file-line">  replicas: 1</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L10" class="blob-num js-line-number js-blob-rnum" data-line-number="10"></td>
          <td id="file-kibana-deployment-yaml-LC10" class="blob-code blob-code-inner js-file-line">  selector:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L11" class="blob-num js-line-number js-blob-rnum" data-line-number="11"></td>
          <td id="file-kibana-deployment-yaml-LC11" class="blob-code blob-code-inner js-file-line">    matchLabels:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L12" class="blob-num js-line-number js-blob-rnum" data-line-number="12"></td>
          <td id="file-kibana-deployment-yaml-LC12" class="blob-code blob-code-inner js-file-line">      app: kibana</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L13" class="blob-num js-line-number js-blob-rnum" data-line-number="13"></td>
          <td id="file-kibana-deployment-yaml-LC13" class="blob-code blob-code-inner js-file-line">  template:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L14" class="blob-num js-line-number js-blob-rnum" data-line-number="14"></td>
          <td id="file-kibana-deployment-yaml-LC14" class="blob-code blob-code-inner js-file-line">    metadata:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L15" class="blob-num js-line-number js-blob-rnum" data-line-number="15"></td>
          <td id="file-kibana-deployment-yaml-LC15" class="blob-code blob-code-inner js-file-line">      labels:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L16" class="blob-num js-line-number js-blob-rnum" data-line-number="16"></td>
          <td id="file-kibana-deployment-yaml-LC16" class="blob-code blob-code-inner js-file-line">        app: kibana</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L17" class="blob-num js-line-number js-blob-rnum" data-line-number="17"></td>
          <td id="file-kibana-deployment-yaml-LC17" class="blob-code blob-code-inner js-file-line">    spec:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L18" class="blob-num js-line-number js-blob-rnum" data-line-number="18"></td>
          <td id="file-kibana-deployment-yaml-LC18" class="blob-code blob-code-inner js-file-line">      containers:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L19" class="blob-num js-line-number js-blob-rnum" data-line-number="19"></td>
          <td id="file-kibana-deployment-yaml-LC19" class="blob-code blob-code-inner js-file-line">      &#8211; name: kibana</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L20" class="blob-num js-line-number js-blob-rnum" data-line-number="20"></td>
          <td id="file-kibana-deployment-yaml-LC20" class="blob-code blob-code-inner js-file-line">        image: docker.elastic.co/kibana/kibana:7.2.0</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L21" class="blob-num js-line-number js-blob-rnum" data-line-number="21"></td>
          <td id="file-kibana-deployment-yaml-LC21" class="blob-code blob-code-inner js-file-line">        resources:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L22" class="blob-num js-line-number js-blob-rnum" data-line-number="22"></td>
          <td id="file-kibana-deployment-yaml-LC22" class="blob-code blob-code-inner js-file-line">          limits:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L23" class="blob-num js-line-number js-blob-rnum" data-line-number="23"></td>
          <td id="file-kibana-deployment-yaml-LC23" class="blob-code blob-code-inner js-file-line">            cpu: 1000m</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L24" class="blob-num js-line-number js-blob-rnum" data-line-number="24"></td>
          <td id="file-kibana-deployment-yaml-LC24" class="blob-code blob-code-inner js-file-line">            memory: 1Gi</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L25" class="blob-num js-line-number js-blob-rnum" data-line-number="25"></td>
          <td id="file-kibana-deployment-yaml-LC25" class="blob-code blob-code-inner js-file-line">          requests:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L26" class="blob-num js-line-number js-blob-rnum" data-line-number="26"></td>
          <td id="file-kibana-deployment-yaml-LC26" class="blob-code blob-code-inner js-file-line">            cpu: 700m</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L27" class="blob-num js-line-number js-blob-rnum" data-line-number="27"></td>
          <td id="file-kibana-deployment-yaml-LC27" class="blob-code blob-code-inner js-file-line">            memory: 1Gi</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L28" class="blob-num js-line-number js-blob-rnum" data-line-number="28"></td>
          <td id="file-kibana-deployment-yaml-LC28" class="blob-code blob-code-inner js-file-line">        env:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L29" class="blob-num js-line-number js-blob-rnum" data-line-number="29"></td>
          <td id="file-kibana-deployment-yaml-LC29" class="blob-code blob-code-inner js-file-line">          &#8211; name: ELASTICSEARCH_URL</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L30" class="blob-num js-line-number js-blob-rnum" data-line-number="30"></td>
          <td id="file-kibana-deployment-yaml-LC30" class="blob-code blob-code-inner js-file-line">            value: http://elasticsearch:9200</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L31" class="blob-num js-line-number js-blob-rnum" data-line-number="31"></td>
          <td id="file-kibana-deployment-yaml-LC31" class="blob-code blob-code-inner js-file-line">          &#8211; name: ELASTICSEARCH_USERNAME</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L32" class="blob-num js-line-number js-blob-rnum" data-line-number="32"></td>
          <td id="file-kibana-deployment-yaml-LC32" class="blob-code blob-code-inner js-file-line">            value: elastic</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L33" class="blob-num js-line-number js-blob-rnum" data-line-number="33"></td>
          <td id="file-kibana-deployment-yaml-LC33" class="blob-code blob-code-inner js-file-line">          &#8211; name: ELASTICSEARCH_PASSWORD</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L34" class="blob-num js-line-number js-blob-rnum" data-line-number="34"></td>
          <td id="file-kibana-deployment-yaml-LC34" class="blob-code blob-code-inner js-file-line">            valueFrom:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L35" class="blob-num js-line-number js-blob-rnum" data-line-number="35"></td>
          <td id="file-kibana-deployment-yaml-LC35" class="blob-code blob-code-inner js-file-line">              secretKeyRef:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L36" class="blob-num js-line-number js-blob-rnum" data-line-number="36"></td>
          <td id="file-kibana-deployment-yaml-LC36" class="blob-code blob-code-inner js-file-line">                name: kibana-password</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L37" class="blob-num js-line-number js-blob-rnum" data-line-number="37"></td>
          <td id="file-kibana-deployment-yaml-LC37" class="blob-code blob-code-inner js-file-line">                key: password</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L38" class="blob-num js-line-number js-blob-rnum" data-line-number="38"></td>
          <td id="file-kibana-deployment-yaml-LC38" class="blob-code blob-code-inner js-file-line">        ports:</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L39" class="blob-num js-line-number js-blob-rnum" data-line-number="39"></td>
          <td id="file-kibana-deployment-yaml-LC39" class="blob-code blob-code-inner js-file-line">        &#8211; containerPort: 5601</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L40" class="blob-num js-line-number js-blob-rnum" data-line-number="40"></td>
          <td id="file-kibana-deployment-yaml-LC40" class="blob-code blob-code-inner js-file-line">          name: kibana</td>
        </tr>
        <tr>
          <td id="file-kibana-deployment-yaml-L41" class="blob-num js-line-number js-blob-rnum" data-line-number="41"></td>
          <td id="file-kibana-deployment-yaml-LC41" class="blob-code blob-code-inner js-file-line">          protocol: TCP</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/87d4d4fbe0e68f2de8ece136b86dfd58/raw/4e1d8eb380b197bd66a4a2cd5a11be22490c4ec1/kibana-deployment.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/87d4d4fbe0e68f2de8ece136b86dfd58#file-kibana-deployment-yaml" class="Link--inTextBlock">
          kibana-deployment.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<h5 class="wp-block-heading">Verify Kibana Deployment</h5>



<p>Let’s check all Kibana pods come into the running state.</p>



<pre class="wp-block-preformatted has-white-color has-dark-gray-background-color has-text-color has-background">kubectl get pods -n kube-logging</pre>



<p class="has-text-align-justify">Once the pods for Elasticsearch, Fluent-bit, and Kibana have entered the running state, you can verify the deployment by accessing the Kibana UI.</p>



<p class="has-text-align-justify">To check the status using the UI access of the cluster, you can use the <code>kubectl port-forward</code> command to forward the Kibana pod&#8217;s 5601 port to your local machine.</p>



<pre class="wp-block-preformatted has-white-color has-dark-gray-background-color has-text-color has-background">$ kubectl port-forward &lt;kibana-pod-name&gt; 5601:5601</pre>



<p>After that, use curl to send a request or the web browser to access the UI.</p>



<pre class="wp-block-preformatted has-white-color has-dark-gray-background-color has-text-color has-background">$ curl http://localhost:5601</pre>



<p>Now that we have a Kibana pod running, let’s move on to fluent-bit.</p>



<h3 class="wp-block-heading">Deploy Fluent-bit as D<strong>aemonset</strong></h3>



<p>Since Fluent-bit must stream logs from every node in the clusters, it is set up as a daemonset. A DaemonSet is a type of Kubernetes resource that ensures that a specified pod is running on each node in the cluster. </p>



<h5 class="wp-block-heading has-text-align-left">Creating the Fluent-bit Service Account</h5>



<p class="has-text-align-justify">A Service Account is a Kubernetes resource that allows you to control access to the Kubernetes API for a set of pods, which determines what the pods are allowed to do. You can attach roles and role bindings to the service account, to give it specific permissions to access the Kubernetes API, this is done through Kubernetes Role and Rolebinding resources.</p>



<p>Let’s create a Service Account for Fluent-bit.</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120370961" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-fluent-bit-sa-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="fluent-bit-sa.yaml">
        <tr>
          <td id="file-fluent-bit-sa-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-fluent-bit-sa-yaml-LC1" class="blob-code blob-code-inner js-file-line">apiVersion: v1</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-sa-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-fluent-bit-sa-yaml-LC2" class="blob-code blob-code-inner js-file-line">kind: ServiceAccount</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-sa-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-fluent-bit-sa-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-sa-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-fluent-bit-sa-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: fluent-bit-sa</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-sa-yaml-L5" class="blob-num js-line-number js-blob-rnum" data-line-number="5"></td>
          <td id="file-fluent-bit-sa-yaml-LC5" class="blob-code blob-code-inner js-file-line">  namespace: kube-logging</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-sa-yaml-L6" class="blob-num js-line-number js-blob-rnum" data-line-number="6"></td>
          <td id="file-fluent-bit-sa-yaml-LC6" class="blob-code blob-code-inner js-file-line">  labels:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-sa-yaml-L7" class="blob-num js-line-number js-blob-rnum" data-line-number="7"></td>
          <td id="file-fluent-bit-sa-yaml-LC7" class="blob-code blob-code-inner js-file-line">    app: fluent-bit-sa</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/af93fc0209138f84c8afb5d9fe5ed33b/raw/0cace24734f041017bbaf37964167eda66d0f9a5/fluent-bit-sa.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/af93fc0209138f84c8afb5d9fe5ed33b#file-fluent-bit-sa-yaml" class="Link--inTextBlock">
          fluent-bit-sa.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<h5 class="wp-block-heading">C<strong>reating the Fluent-bit ClusterRole</strong> </h5>



<p class="has-text-align-justify">ClusterRole to grant the <code>get</code>, <code>list</code>, and <code>watch</code> permissions to fluent-bit Service Account on the Kubernetes resources like the <strong>nodes</strong>, <strong>pods</strong> and <strong>namespaces</strong> objects.</p>



<p>Let’s create a ClusterRole for  Fluent-bit.</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120370995" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-fluent-bit-role-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="fluent-bit-role.yaml">
        <tr>
          <td id="file-fluent-bit-role-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-fluent-bit-role-yaml-LC1" class="blob-code blob-code-inner js-file-line">apiVersion: rbac.authorization.k8s.io/v1</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-fluent-bit-role-yaml-LC2" class="blob-code blob-code-inner js-file-line">kind: ClusterRole</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-fluent-bit-role-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-fluent-bit-role-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: fluent-bit-role</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L5" class="blob-num js-line-number js-blob-rnum" data-line-number="5"></td>
          <td id="file-fluent-bit-role-yaml-LC5" class="blob-code blob-code-inner js-file-line">  labels:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L6" class="blob-num js-line-number js-blob-rnum" data-line-number="6"></td>
          <td id="file-fluent-bit-role-yaml-LC6" class="blob-code blob-code-inner js-file-line">    app: fluent-bit-role</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L7" class="blob-num js-line-number js-blob-rnum" data-line-number="7"></td>
          <td id="file-fluent-bit-role-yaml-LC7" class="blob-code blob-code-inner js-file-line">rules:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L8" class="blob-num js-line-number js-blob-rnum" data-line-number="8"></td>
          <td id="file-fluent-bit-role-yaml-LC8" class="blob-code blob-code-inner js-file-line">  &#8211; nonResourceURLs:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L9" class="blob-num js-line-number js-blob-rnum" data-line-number="9"></td>
          <td id="file-fluent-bit-role-yaml-LC9" class="blob-code blob-code-inner js-file-line">      &#8211; /metrics</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L10" class="blob-num js-line-number js-blob-rnum" data-line-number="10"></td>
          <td id="file-fluent-bit-role-yaml-LC10" class="blob-code blob-code-inner js-file-line">    verbs:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L11" class="blob-num js-line-number js-blob-rnum" data-line-number="11"></td>
          <td id="file-fluent-bit-role-yaml-LC11" class="blob-code blob-code-inner js-file-line">      &#8211; get</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L12" class="blob-num js-line-number js-blob-rnum" data-line-number="12"></td>
          <td id="file-fluent-bit-role-yaml-LC12" class="blob-code blob-code-inner js-file-line">  &#8211; apiGroups: [&quot;&quot;]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L13" class="blob-num js-line-number js-blob-rnum" data-line-number="13"></td>
          <td id="file-fluent-bit-role-yaml-LC13" class="blob-code blob-code-inner js-file-line">    resources:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L14" class="blob-num js-line-number js-blob-rnum" data-line-number="14"></td>
          <td id="file-fluent-bit-role-yaml-LC14" class="blob-code blob-code-inner js-file-line">      &#8211; namespaces</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L15" class="blob-num js-line-number js-blob-rnum" data-line-number="15"></td>
          <td id="file-fluent-bit-role-yaml-LC15" class="blob-code blob-code-inner js-file-line">      &#8211; pods</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L16" class="blob-num js-line-number js-blob-rnum" data-line-number="16"></td>
          <td id="file-fluent-bit-role-yaml-LC16" class="blob-code blob-code-inner js-file-line">      &#8211; pods/logs</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-role-yaml-L17" class="blob-num js-line-number js-blob-rnum" data-line-number="17"></td>
          <td id="file-fluent-bit-role-yaml-LC17" class="blob-code blob-code-inner js-file-line">    verbs: [&quot;get&quot;, &quot;list&quot;, &quot;watch&quot;]</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/173d24d28b8da3b9d58acb42fa7dea7d/raw/3ebf55b9e92d354e14fc6c0f2b4a150302e3df47/fluent-bit-role.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/173d24d28b8da3b9d58acb42fa7dea7d#file-fluent-bit-role-yaml" class="Link--inTextBlock">
          fluent-bit-role.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<h5 class="wp-block-heading">Creating the Fluent-bit Role Binding</h5>



<p class="has-text-align-justify">ClusterRoleBinding to bind this ClusterRole to the “fluent-bit-sa” Service Account, which will give that ServiceAccount the permissions defined in the ClusterRole. </p>



<p>Let’s create a Role Binding for Fluent-bit.</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120371007" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-fluent-bit-rb-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="fluent-bit-rb.yaml">
        <tr>
          <td id="file-fluent-bit-rb-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-fluent-bit-rb-yaml-LC1" class="blob-code blob-code-inner js-file-line">kind: ClusterRoleBinding</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-rb-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-fluent-bit-rb-yaml-LC2" class="blob-code blob-code-inner js-file-line">apiVersion: rbac.authorization.k8s.io/v1</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-rb-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-fluent-bit-rb-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-rb-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-fluent-bit-rb-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: fluent-bit-rb</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-rb-yaml-L5" class="blob-num js-line-number js-blob-rnum" data-line-number="5"></td>
          <td id="file-fluent-bit-rb-yaml-LC5" class="blob-code blob-code-inner js-file-line">roleRef:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-rb-yaml-L6" class="blob-num js-line-number js-blob-rnum" data-line-number="6"></td>
          <td id="file-fluent-bit-rb-yaml-LC6" class="blob-code blob-code-inner js-file-line">  kind: ClusterRole</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-rb-yaml-L7" class="blob-num js-line-number js-blob-rnum" data-line-number="7"></td>
          <td id="file-fluent-bit-rb-yaml-LC7" class="blob-code blob-code-inner js-file-line">  name: fluent-bit-role</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-rb-yaml-L8" class="blob-num js-line-number js-blob-rnum" data-line-number="8"></td>
          <td id="file-fluent-bit-rb-yaml-LC8" class="blob-code blob-code-inner js-file-line">  apiGroup: rbac.authorization.k8s.io</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-rb-yaml-L9" class="blob-num js-line-number js-blob-rnum" data-line-number="9"></td>
          <td id="file-fluent-bit-rb-yaml-LC9" class="blob-code blob-code-inner js-file-line">subjects:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-rb-yaml-L10" class="blob-num js-line-number js-blob-rnum" data-line-number="10"></td>
          <td id="file-fluent-bit-rb-yaml-LC10" class="blob-code blob-code-inner js-file-line">&#8211; kind: ServiceAccount</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-rb-yaml-L11" class="blob-num js-line-number js-blob-rnum" data-line-number="11"></td>
          <td id="file-fluent-bit-rb-yaml-LC11" class="blob-code blob-code-inner js-file-line">  name: fluent-bit-sa</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-rb-yaml-L12" class="blob-num js-line-number js-blob-rnum" data-line-number="12"></td>
          <td id="file-fluent-bit-rb-yaml-LC12" class="blob-code blob-code-inner js-file-line">  namespace: kube-logging</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/f6ee45df61e168ea2912db2747b2439d/raw/ecb364d99744d21c390bd4f10a1aae2a7ab3cf5f/fluent-bit-rb.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/f6ee45df61e168ea2912db2747b2439d#file-fluent-bit-rb-yaml" class="Link--inTextBlock">
          fluent-bit-rb.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<h5 class="wp-block-heading has-text-align-justify">Creating the Fluent-bit ConfigMap</h5>



<p class="has-text-align-justify has-dark-gray-color has-white-background-color has-text-color has-background">This ConfigMap is used to configure a Fluent-bit pod, by specifying the ConfigMap field in the pod definition. This way when the pod starts it will use the configurations defined in the configmap. ConfigMap can be updates and changes, it will reflect in the pod without the need to recreate the pod itself. </p>



<p>Let’s create a ConfigMap for Fluent-bit.</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120371023" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-fluent-bit-configmap-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="fluent-bit-configmap.yaml">
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-fluent-bit-configmap-yaml-LC1" class="blob-code blob-code-inner js-file-line">apiVersion: v1</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-fluent-bit-configmap-yaml-LC2" class="blob-code blob-code-inner js-file-line">kind: ConfigMap</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-fluent-bit-configmap-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-fluent-bit-configmap-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: fluent-bit-config</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L5" class="blob-num js-line-number js-blob-rnum" data-line-number="5"></td>
          <td id="file-fluent-bit-configmap-yaml-LC5" class="blob-code blob-code-inner js-file-line">  namespace: kube-logging</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L6" class="blob-num js-line-number js-blob-rnum" data-line-number="6"></td>
          <td id="file-fluent-bit-configmap-yaml-LC6" class="blob-code blob-code-inner js-file-line">  labels:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L7" class="blob-num js-line-number js-blob-rnum" data-line-number="7"></td>
          <td id="file-fluent-bit-configmap-yaml-LC7" class="blob-code blob-code-inner js-file-line">    k8s-app: f-bit-pod</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L8" class="blob-num js-line-number js-blob-rnum" data-line-number="8"></td>
          <td id="file-fluent-bit-configmap-yaml-LC8" class="blob-code blob-code-inner js-file-line">data:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L9" class="blob-num js-line-number js-blob-rnum" data-line-number="9"></td>
          <td id="file-fluent-bit-configmap-yaml-LC9" class="blob-code blob-code-inner js-file-line">  # Configuration files: server, input, filters and output</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L10" class="blob-num js-line-number js-blob-rnum" data-line-number="10"></td>
          <td id="file-fluent-bit-configmap-yaml-LC10" class="blob-code blob-code-inner js-file-line">  # ======================================================</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L11" class="blob-num js-line-number js-blob-rnum" data-line-number="11"></td>
          <td id="file-fluent-bit-configmap-yaml-LC11" class="blob-code blob-code-inner js-file-line">  fluent-bit.conf: |</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L12" class="blob-num js-line-number js-blob-rnum" data-line-number="12"></td>
          <td id="file-fluent-bit-configmap-yaml-LC12" class="blob-code blob-code-inner js-file-line">    [SERVICE]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L13" class="blob-num js-line-number js-blob-rnum" data-line-number="13"></td>
          <td id="file-fluent-bit-configmap-yaml-LC13" class="blob-code blob-code-inner js-file-line">        Flush         1</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L14" class="blob-num js-line-number js-blob-rnum" data-line-number="14"></td>
          <td id="file-fluent-bit-configmap-yaml-LC14" class="blob-code blob-code-inner js-file-line">        Log_Level     info</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L15" class="blob-num js-line-number js-blob-rnum" data-line-number="15"></td>
          <td id="file-fluent-bit-configmap-yaml-LC15" class="blob-code blob-code-inner js-file-line">        Daemon        off</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L16" class="blob-num js-line-number js-blob-rnum" data-line-number="16"></td>
          <td id="file-fluent-bit-configmap-yaml-LC16" class="blob-code blob-code-inner js-file-line">        Parsers_File  parsers.conf</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L17" class="blob-num js-line-number js-blob-rnum" data-line-number="17"></td>
          <td id="file-fluent-bit-configmap-yaml-LC17" class="blob-code blob-code-inner js-file-line">        HTTP_Server   On</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L18" class="blob-num js-line-number js-blob-rnum" data-line-number="18"></td>
          <td id="file-fluent-bit-configmap-yaml-LC18" class="blob-code blob-code-inner js-file-line">        HTTP_Listen   0.0.0.0</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L19" class="blob-num js-line-number js-blob-rnum" data-line-number="19"></td>
          <td id="file-fluent-bit-configmap-yaml-LC19" class="blob-code blob-code-inner js-file-line">        HTTP_Port     2020</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L20" class="blob-num js-line-number js-blob-rnum" data-line-number="20"></td>
          <td id="file-fluent-bit-configmap-yaml-LC20" class="blob-code blob-code-inner js-file-line">
</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L21" class="blob-num js-line-number js-blob-rnum" data-line-number="21"></td>
          <td id="file-fluent-bit-configmap-yaml-LC21" class="blob-code blob-code-inner js-file-line">    @INCLUDE input-kubernetes.conf</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L22" class="blob-num js-line-number js-blob-rnum" data-line-number="22"></td>
          <td id="file-fluent-bit-configmap-yaml-LC22" class="blob-code blob-code-inner js-file-line">    @INCLUDE filter-kubernetes.conf</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L23" class="blob-num js-line-number js-blob-rnum" data-line-number="23"></td>
          <td id="file-fluent-bit-configmap-yaml-LC23" class="blob-code blob-code-inner js-file-line">    @INCLUDE output-elasticsearch.conf</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L24" class="blob-num js-line-number js-blob-rnum" data-line-number="24"></td>
          <td id="file-fluent-bit-configmap-yaml-LC24" class="blob-code blob-code-inner js-file-line">
</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L25" class="blob-num js-line-number js-blob-rnum" data-line-number="25"></td>
          <td id="file-fluent-bit-configmap-yaml-LC25" class="blob-code blob-code-inner js-file-line">  input-kubernetes.conf: |</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L26" class="blob-num js-line-number js-blob-rnum" data-line-number="26"></td>
          <td id="file-fluent-bit-configmap-yaml-LC26" class="blob-code blob-code-inner js-file-line">    [INPUT]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L27" class="blob-num js-line-number js-blob-rnum" data-line-number="27"></td>
          <td id="file-fluent-bit-configmap-yaml-LC27" class="blob-code blob-code-inner js-file-line">        Name              tail</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L28" class="blob-num js-line-number js-blob-rnum" data-line-number="28"></td>
          <td id="file-fluent-bit-configmap-yaml-LC28" class="blob-code blob-code-inner js-file-line">        Tag               kube.*</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L29" class="blob-num js-line-number js-blob-rnum" data-line-number="29"></td>
          <td id="file-fluent-bit-configmap-yaml-LC29" class="blob-code blob-code-inner js-file-line">        Path              /var/log/containers/*.log</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L30" class="blob-num js-line-number js-blob-rnum" data-line-number="30"></td>
          <td id="file-fluent-bit-configmap-yaml-LC30" class="blob-code blob-code-inner js-file-line">        Parser            docker</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L31" class="blob-num js-line-number js-blob-rnum" data-line-number="31"></td>
          <td id="file-fluent-bit-configmap-yaml-LC31" class="blob-code blob-code-inner js-file-line">        DB                /var/log/flb_kube.db</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L32" class="blob-num js-line-number js-blob-rnum" data-line-number="32"></td>
          <td id="file-fluent-bit-configmap-yaml-LC32" class="blob-code blob-code-inner js-file-line">        Mem_Buf_Limit     5MB</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L33" class="blob-num js-line-number js-blob-rnum" data-line-number="33"></td>
          <td id="file-fluent-bit-configmap-yaml-LC33" class="blob-code blob-code-inner js-file-line">        Skip_Long_Lines   On</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L34" class="blob-num js-line-number js-blob-rnum" data-line-number="34"></td>
          <td id="file-fluent-bit-configmap-yaml-LC34" class="blob-code blob-code-inner js-file-line">        Refresh_Interval  10</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L35" class="blob-num js-line-number js-blob-rnum" data-line-number="35"></td>
          <td id="file-fluent-bit-configmap-yaml-LC35" class="blob-code blob-code-inner js-file-line">
</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L36" class="blob-num js-line-number js-blob-rnum" data-line-number="36"></td>
          <td id="file-fluent-bit-configmap-yaml-LC36" class="blob-code blob-code-inner js-file-line">  filter-kubernetes.conf: |</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L37" class="blob-num js-line-number js-blob-rnum" data-line-number="37"></td>
          <td id="file-fluent-bit-configmap-yaml-LC37" class="blob-code blob-code-inner js-file-line">    [FILTER]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L38" class="blob-num js-line-number js-blob-rnum" data-line-number="38"></td>
          <td id="file-fluent-bit-configmap-yaml-LC38" class="blob-code blob-code-inner js-file-line">        Name                kubernetes</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L39" class="blob-num js-line-number js-blob-rnum" data-line-number="39"></td>
          <td id="file-fluent-bit-configmap-yaml-LC39" class="blob-code blob-code-inner js-file-line">        Match               kube.*</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L40" class="blob-num js-line-number js-blob-rnum" data-line-number="40"></td>
          <td id="file-fluent-bit-configmap-yaml-LC40" class="blob-code blob-code-inner js-file-line">        Kube_URL            https://kubernetes.default.svc:443</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L41" class="blob-num js-line-number js-blob-rnum" data-line-number="41"></td>
          <td id="file-fluent-bit-configmap-yaml-LC41" class="blob-code blob-code-inner js-file-line">        Kube_CA_File        /var/run/secrets/kubernetes.io/serviceaccount/ca.crt</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L42" class="blob-num js-line-number js-blob-rnum" data-line-number="42"></td>
          <td id="file-fluent-bit-configmap-yaml-LC42" class="blob-code blob-code-inner js-file-line">        Kube_Token_File     /var/run/secrets/kubernetes.io/serviceaccount/token</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L43" class="blob-num js-line-number js-blob-rnum" data-line-number="43"></td>
          <td id="file-fluent-bit-configmap-yaml-LC43" class="blob-code blob-code-inner js-file-line">        Kube_Tag_Prefix     kube.var.log.containers.*</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L44" class="blob-num js-line-number js-blob-rnum" data-line-number="44"></td>
          <td id="file-fluent-bit-configmap-yaml-LC44" class="blob-code blob-code-inner js-file-line">        Merge_Log           On</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L45" class="blob-num js-line-number js-blob-rnum" data-line-number="45"></td>
          <td id="file-fluent-bit-configmap-yaml-LC45" class="blob-code blob-code-inner js-file-line">        Merge_Log_Key       log_processed</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L46" class="blob-num js-line-number js-blob-rnum" data-line-number="46"></td>
          <td id="file-fluent-bit-configmap-yaml-LC46" class="blob-code blob-code-inner js-file-line">        K8S-Logging.Parser  On</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L47" class="blob-num js-line-number js-blob-rnum" data-line-number="47"></td>
          <td id="file-fluent-bit-configmap-yaml-LC47" class="blob-code blob-code-inner js-file-line">        K8S-Logging.Exclude Off</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L48" class="blob-num js-line-number js-blob-rnum" data-line-number="48"></td>
          <td id="file-fluent-bit-configmap-yaml-LC48" class="blob-code blob-code-inner js-file-line">
</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L49" class="blob-num js-line-number js-blob-rnum" data-line-number="49"></td>
          <td id="file-fluent-bit-configmap-yaml-LC49" class="blob-code blob-code-inner js-file-line">  output-elasticsearch.conf: |</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L50" class="blob-num js-line-number js-blob-rnum" data-line-number="50"></td>
          <td id="file-fluent-bit-configmap-yaml-LC50" class="blob-code blob-code-inner js-file-line">    [OUTPUT]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L51" class="blob-num js-line-number js-blob-rnum" data-line-number="51"></td>
          <td id="file-fluent-bit-configmap-yaml-LC51" class="blob-code blob-code-inner js-file-line">        Name            es</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L52" class="blob-num js-line-number js-blob-rnum" data-line-number="52"></td>
          <td id="file-fluent-bit-configmap-yaml-LC52" class="blob-code blob-code-inner js-file-line">        Match           *</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L53" class="blob-num js-line-number js-blob-rnum" data-line-number="53"></td>
          <td id="file-fluent-bit-configmap-yaml-LC53" class="blob-code blob-code-inner js-file-line">        Host            ${FLUENT_ELASTICSEARCH_HOST}</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L54" class="blob-num js-line-number js-blob-rnum" data-line-number="54"></td>
          <td id="file-fluent-bit-configmap-yaml-LC54" class="blob-code blob-code-inner js-file-line">        Port            ${FLUENT_ELASTICSEARCH_PORT}</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L55" class="blob-num js-line-number js-blob-rnum" data-line-number="55"></td>
          <td id="file-fluent-bit-configmap-yaml-LC55" class="blob-code blob-code-inner js-file-line">        HTTP_User       ${FLUENT_ELASTICSEARCH_USER}</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L56" class="blob-num js-line-number js-blob-rnum" data-line-number="56"></td>
          <td id="file-fluent-bit-configmap-yaml-LC56" class="blob-code blob-code-inner js-file-line">        HTTP_Passwd     ${FLUENT_ELASTICSEARCH_PASSWORD}</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L57" class="blob-num js-line-number js-blob-rnum" data-line-number="57"></td>
          <td id="file-fluent-bit-configmap-yaml-LC57" class="blob-code blob-code-inner js-file-line">        Logstash_Format On</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L58" class="blob-num js-line-number js-blob-rnum" data-line-number="58"></td>
          <td id="file-fluent-bit-configmap-yaml-LC58" class="blob-code blob-code-inner js-file-line">        Replace_Dots    On</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L59" class="blob-num js-line-number js-blob-rnum" data-line-number="59"></td>
          <td id="file-fluent-bit-configmap-yaml-LC59" class="blob-code blob-code-inner js-file-line">        Retry_Limit     False</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L60" class="blob-num js-line-number js-blob-rnum" data-line-number="60"></td>
          <td id="file-fluent-bit-configmap-yaml-LC60" class="blob-code blob-code-inner js-file-line">
</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L61" class="blob-num js-line-number js-blob-rnum" data-line-number="61"></td>
          <td id="file-fluent-bit-configmap-yaml-LC61" class="blob-code blob-code-inner js-file-line">  parsers.conf: |</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L62" class="blob-num js-line-number js-blob-rnum" data-line-number="62"></td>
          <td id="file-fluent-bit-configmap-yaml-LC62" class="blob-code blob-code-inner js-file-line">    [PARSER]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L63" class="blob-num js-line-number js-blob-rnum" data-line-number="63"></td>
          <td id="file-fluent-bit-configmap-yaml-LC63" class="blob-code blob-code-inner js-file-line">        Name   apache</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L64" class="blob-num js-line-number js-blob-rnum" data-line-number="64"></td>
          <td id="file-fluent-bit-configmap-yaml-LC64" class="blob-code blob-code-inner js-file-line">        Format regex</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L65" class="blob-num js-line-number js-blob-rnum" data-line-number="65"></td>
          <td id="file-fluent-bit-configmap-yaml-LC65" class="blob-code blob-code-inner js-file-line">        Regex  ^(?&lt;host&gt;[^ ]*) [^ ]* (?&lt;user&gt;[^ ]*) \[(?&lt;time&gt;[^\]]*)\] &quot;(?&lt;method&gt;\S+)(?: +(?&lt;path&gt;[^\&quot;]*?)(?: +\S*)?)?&quot; (?&lt;code&gt;[^ ]*) (?&lt;size&gt;[^ ]*)(?: &quot;(?&lt;referer&gt;[^\&quot;]*)&quot; &quot;(?&lt;agent&gt;[^\&quot;]*)&quot;)?$</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L66" class="blob-num js-line-number js-blob-rnum" data-line-number="66"></td>
          <td id="file-fluent-bit-configmap-yaml-LC66" class="blob-code blob-code-inner js-file-line">        Time_Key time</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L67" class="blob-num js-line-number js-blob-rnum" data-line-number="67"></td>
          <td id="file-fluent-bit-configmap-yaml-LC67" class="blob-code blob-code-inner js-file-line">        Time_Format %d/%b/%Y:%H:%M:%S %z</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L68" class="blob-num js-line-number js-blob-rnum" data-line-number="68"></td>
          <td id="file-fluent-bit-configmap-yaml-LC68" class="blob-code blob-code-inner js-file-line">
</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L69" class="blob-num js-line-number js-blob-rnum" data-line-number="69"></td>
          <td id="file-fluent-bit-configmap-yaml-LC69" class="blob-code blob-code-inner js-file-line">    [PARSER]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L70" class="blob-num js-line-number js-blob-rnum" data-line-number="70"></td>
          <td id="file-fluent-bit-configmap-yaml-LC70" class="blob-code blob-code-inner js-file-line">        Name   apache2</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L71" class="blob-num js-line-number js-blob-rnum" data-line-number="71"></td>
          <td id="file-fluent-bit-configmap-yaml-LC71" class="blob-code blob-code-inner js-file-line">        Format regex</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L72" class="blob-num js-line-number js-blob-rnum" data-line-number="72"></td>
          <td id="file-fluent-bit-configmap-yaml-LC72" class="blob-code blob-code-inner js-file-line">        Regex  ^(?&lt;host&gt;[^ ]*) [^ ]* (?&lt;user&gt;[^ ]*) \[(?&lt;time&gt;[^\]]*)\] &quot;(?&lt;method&gt;\S+)(?: +(?&lt;path&gt;[^ ]*) +\S*)?&quot; (?&lt;code&gt;[^ ]*) (?&lt;size&gt;[^ ]*)(?: &quot;(?&lt;referer&gt;[^\&quot;]*)&quot; &quot;(?&lt;agent&gt;[^\&quot;]*)&quot;)?$</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L73" class="blob-num js-line-number js-blob-rnum" data-line-number="73"></td>
          <td id="file-fluent-bit-configmap-yaml-LC73" class="blob-code blob-code-inner js-file-line">        Time_Key time</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L74" class="blob-num js-line-number js-blob-rnum" data-line-number="74"></td>
          <td id="file-fluent-bit-configmap-yaml-LC74" class="blob-code blob-code-inner js-file-line">        Time_Format %d/%b/%Y:%H:%M:%S %z</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L75" class="blob-num js-line-number js-blob-rnum" data-line-number="75"></td>
          <td id="file-fluent-bit-configmap-yaml-LC75" class="blob-code blob-code-inner js-file-line">
</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L76" class="blob-num js-line-number js-blob-rnum" data-line-number="76"></td>
          <td id="file-fluent-bit-configmap-yaml-LC76" class="blob-code blob-code-inner js-file-line">    [PARSER]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L77" class="blob-num js-line-number js-blob-rnum" data-line-number="77"></td>
          <td id="file-fluent-bit-configmap-yaml-LC77" class="blob-code blob-code-inner js-file-line">        Name   apache_error</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L78" class="blob-num js-line-number js-blob-rnum" data-line-number="78"></td>
          <td id="file-fluent-bit-configmap-yaml-LC78" class="blob-code blob-code-inner js-file-line">        Format regex</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L79" class="blob-num js-line-number js-blob-rnum" data-line-number="79"></td>
          <td id="file-fluent-bit-configmap-yaml-LC79" class="blob-code blob-code-inner js-file-line">        Regex  ^\[[^ ]* (?&lt;time&gt;[^\]]*)\] \[(?&lt;level&gt;[^\]]*)\](?: \[pid (?&lt;pid&gt;[^\]]*)\])?( \[client (?&lt;client&gt;[^\]]*)\])? (?&lt;message&gt;.*)$</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L80" class="blob-num js-line-number js-blob-rnum" data-line-number="80"></td>
          <td id="file-fluent-bit-configmap-yaml-LC80" class="blob-code blob-code-inner js-file-line">
</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L81" class="blob-num js-line-number js-blob-rnum" data-line-number="81"></td>
          <td id="file-fluent-bit-configmap-yaml-LC81" class="blob-code blob-code-inner js-file-line">    [PARSER]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L82" class="blob-num js-line-number js-blob-rnum" data-line-number="82"></td>
          <td id="file-fluent-bit-configmap-yaml-LC82" class="blob-code blob-code-inner js-file-line">        Name   nginx</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L83" class="blob-num js-line-number js-blob-rnum" data-line-number="83"></td>
          <td id="file-fluent-bit-configmap-yaml-LC83" class="blob-code blob-code-inner js-file-line">        Format regex</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L84" class="blob-num js-line-number js-blob-rnum" data-line-number="84"></td>
          <td id="file-fluent-bit-configmap-yaml-LC84" class="blob-code blob-code-inner js-file-line">        Regex ^(?&lt;remote&gt;[^ ]*) (?&lt;host&gt;[^ ]*) (?&lt;user&gt;[^ ]*) \[(?&lt;time&gt;[^\]]*)\] &quot;(?&lt;method&gt;\S+)(?: +(?&lt;path&gt;[^\&quot;]*?)(?: +\S*)?)?&quot; (?&lt;code&gt;[^ ]*) (?&lt;size&gt;[^ ]*)(?: &quot;(?&lt;referer&gt;[^\&quot;]*)&quot; &quot;(?&lt;agent&gt;[^\&quot;]*)&quot;)?$</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L85" class="blob-num js-line-number js-blob-rnum" data-line-number="85"></td>
          <td id="file-fluent-bit-configmap-yaml-LC85" class="blob-code blob-code-inner js-file-line">        Time_Key time</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L86" class="blob-num js-line-number js-blob-rnum" data-line-number="86"></td>
          <td id="file-fluent-bit-configmap-yaml-LC86" class="blob-code blob-code-inner js-file-line">        Time_Format %d/%b/%Y:%H:%M:%S %z</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L87" class="blob-num js-line-number js-blob-rnum" data-line-number="87"></td>
          <td id="file-fluent-bit-configmap-yaml-LC87" class="blob-code blob-code-inner js-file-line">
</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L88" class="blob-num js-line-number js-blob-rnum" data-line-number="88"></td>
          <td id="file-fluent-bit-configmap-yaml-LC88" class="blob-code blob-code-inner js-file-line">    [PARSER]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L89" class="blob-num js-line-number js-blob-rnum" data-line-number="89"></td>
          <td id="file-fluent-bit-configmap-yaml-LC89" class="blob-code blob-code-inner js-file-line">        Name   json</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L90" class="blob-num js-line-number js-blob-rnum" data-line-number="90"></td>
          <td id="file-fluent-bit-configmap-yaml-LC90" class="blob-code blob-code-inner js-file-line">        Format json</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L91" class="blob-num js-line-number js-blob-rnum" data-line-number="91"></td>
          <td id="file-fluent-bit-configmap-yaml-LC91" class="blob-code blob-code-inner js-file-line">        Time_Key time</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L92" class="blob-num js-line-number js-blob-rnum" data-line-number="92"></td>
          <td id="file-fluent-bit-configmap-yaml-LC92" class="blob-code blob-code-inner js-file-line">        Time_Format %d/%b/%Y:%H:%M:%S %z</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L93" class="blob-num js-line-number js-blob-rnum" data-line-number="93"></td>
          <td id="file-fluent-bit-configmap-yaml-LC93" class="blob-code blob-code-inner js-file-line">
</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L94" class="blob-num js-line-number js-blob-rnum" data-line-number="94"></td>
          <td id="file-fluent-bit-configmap-yaml-LC94" class="blob-code blob-code-inner js-file-line">    [PARSER]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L95" class="blob-num js-line-number js-blob-rnum" data-line-number="95"></td>
          <td id="file-fluent-bit-configmap-yaml-LC95" class="blob-code blob-code-inner js-file-line">        Name        docker</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L96" class="blob-num js-line-number js-blob-rnum" data-line-number="96"></td>
          <td id="file-fluent-bit-configmap-yaml-LC96" class="blob-code blob-code-inner js-file-line">        Format      json</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L97" class="blob-num js-line-number js-blob-rnum" data-line-number="97"></td>
          <td id="file-fluent-bit-configmap-yaml-LC97" class="blob-code blob-code-inner js-file-line">        Time_Key    time</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L98" class="blob-num js-line-number js-blob-rnum" data-line-number="98"></td>
          <td id="file-fluent-bit-configmap-yaml-LC98" class="blob-code blob-code-inner js-file-line">        Time_Format %Y-%m-%dT%H:%M:%S.%L</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L99" class="blob-num js-line-number js-blob-rnum" data-line-number="99"></td>
          <td id="file-fluent-bit-configmap-yaml-LC99" class="blob-code blob-code-inner js-file-line">        Time_Keep   On</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L100" class="blob-num js-line-number js-blob-rnum" data-line-number="100"></td>
          <td id="file-fluent-bit-configmap-yaml-LC100" class="blob-code blob-code-inner js-file-line">
</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L101" class="blob-num js-line-number js-blob-rnum" data-line-number="101"></td>
          <td id="file-fluent-bit-configmap-yaml-LC101" class="blob-code blob-code-inner js-file-line">    [PARSER]</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L102" class="blob-num js-line-number js-blob-rnum" data-line-number="102"></td>
          <td id="file-fluent-bit-configmap-yaml-LC102" class="blob-code blob-code-inner js-file-line">        Name        syslog</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L103" class="blob-num js-line-number js-blob-rnum" data-line-number="103"></td>
          <td id="file-fluent-bit-configmap-yaml-LC103" class="blob-code blob-code-inner js-file-line">        Format      regex</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L104" class="blob-num js-line-number js-blob-rnum" data-line-number="104"></td>
          <td id="file-fluent-bit-configmap-yaml-LC104" class="blob-code blob-code-inner js-file-line">        Regex       ^\&lt;(?&lt;pri&gt;[0-9]+)\&gt;(?&lt;time&gt;[^ ]* {1,2}[^ ]* [^ ]*) (?&lt;host&gt;[^ ]*) (?&lt;ident&gt;[a-zA-Z0-9_\/\.\-]*)(?:\[(?&lt;pid&gt;[0-9]+)\])?(?:[^\:]*\:)? *(?&lt;message&gt;.*)$</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L105" class="blob-num js-line-number js-blob-rnum" data-line-number="105"></td>
          <td id="file-fluent-bit-configmap-yaml-LC105" class="blob-code blob-code-inner js-file-line">        Time_Key    time</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-configmap-yaml-L106" class="blob-num js-line-number js-blob-rnum" data-line-number="106"></td>
          <td id="file-fluent-bit-configmap-yaml-LC106" class="blob-code blob-code-inner js-file-line">        Time_Format %b %d %H:%M:%S</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/80ac75258dfc09ff3fd7ff505c70b6c1/raw/1ed1904510eaf3d2a6cdb3d8b5d5a87518d2a859/fluent-bit-configmap.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/80ac75258dfc09ff3fd7ff505c70b6c1#file-fluent-bit-configmap-yaml" class="Link--inTextBlock">
          fluent-bit-configmap.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<p>The configuration in this manifest file specifies several different sections, including:</p>



<ul>
<li>[SERVICE]: in which we specify the flush time and log level of the service.</li>



<li>[INPUT]: in which we specify the input plugin that Fluent-bit should use to collect log data. In this case, the plugin is “tail”, which is used to collect logs from the files at the specified path, here is “/var/log/containers/*.log”</li>



<li>[FILTER]: in which we specify the filter plugin that Fluent-bit should use to process the log data. Here is “kubernetes” which is used to parse Kubernetes-specific metadata from the log lines.</li>



<li>[OUTPUT]: in which we specify the output plugin that Fluent-bit should use to send log data to Elasticsearch. It also includes the Elasticsearch endpoint host and port, along with other configurations.</li>



<li>[PARSER] in which we specify the format of the log data and how it should be parsed.</li>
</ul>



<h5 class="wp-block-heading has-text-align-justify">Creating the Fluent-bit Daemonset</h5>



<p class="has-text-align-justify">The Fluent-bit Daemonset will automatically start collecting logs from all the nodes and send them to Elasticsearch.</p>



<p>Let’s create a Daemonset for Fluent-bit.</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120371065" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-fluent-bit-ds-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="fluent-bit-ds.yaml">
        <tr>
          <td id="file-fluent-bit-ds-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-fluent-bit-ds-yaml-LC1" class="blob-code blob-code-inner js-file-line">apiVersion: apps/v1</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-fluent-bit-ds-yaml-LC2" class="blob-code blob-code-inner js-file-line">kind: DaemonSet</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-fluent-bit-ds-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-fluent-bit-ds-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: f-bit-pod</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L5" class="blob-num js-line-number js-blob-rnum" data-line-number="5"></td>
          <td id="file-fluent-bit-ds-yaml-LC5" class="blob-code blob-code-inner js-file-line">  namespace: kube-logging</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L6" class="blob-num js-line-number js-blob-rnum" data-line-number="6"></td>
          <td id="file-fluent-bit-ds-yaml-LC6" class="blob-code blob-code-inner js-file-line">  labels:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L7" class="blob-num js-line-number js-blob-rnum" data-line-number="7"></td>
          <td id="file-fluent-bit-ds-yaml-LC7" class="blob-code blob-code-inner js-file-line">    k8s-app: fluent-bit-logging</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L8" class="blob-num js-line-number js-blob-rnum" data-line-number="8"></td>
          <td id="file-fluent-bit-ds-yaml-LC8" class="blob-code blob-code-inner js-file-line">    version: v1</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L9" class="blob-num js-line-number js-blob-rnum" data-line-number="9"></td>
          <td id="file-fluent-bit-ds-yaml-LC9" class="blob-code blob-code-inner js-file-line">    kubernetes.io/cluster-service: &quot;true&quot;</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L10" class="blob-num js-line-number js-blob-rnum" data-line-number="10"></td>
          <td id="file-fluent-bit-ds-yaml-LC10" class="blob-code blob-code-inner js-file-line">spec:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L11" class="blob-num js-line-number js-blob-rnum" data-line-number="11"></td>
          <td id="file-fluent-bit-ds-yaml-LC11" class="blob-code blob-code-inner js-file-line">  selector:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L12" class="blob-num js-line-number js-blob-rnum" data-line-number="12"></td>
          <td id="file-fluent-bit-ds-yaml-LC12" class="blob-code blob-code-inner js-file-line">    matchLabels:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L13" class="blob-num js-line-number js-blob-rnum" data-line-number="13"></td>
          <td id="file-fluent-bit-ds-yaml-LC13" class="blob-code blob-code-inner js-file-line">      k8s-app: fluent-bit-logging</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L14" class="blob-num js-line-number js-blob-rnum" data-line-number="14"></td>
          <td id="file-fluent-bit-ds-yaml-LC14" class="blob-code blob-code-inner js-file-line">  template:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L15" class="blob-num js-line-number js-blob-rnum" data-line-number="15"></td>
          <td id="file-fluent-bit-ds-yaml-LC15" class="blob-code blob-code-inner js-file-line">    metadata:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L16" class="blob-num js-line-number js-blob-rnum" data-line-number="16"></td>
          <td id="file-fluent-bit-ds-yaml-LC16" class="blob-code blob-code-inner js-file-line">      labels:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L17" class="blob-num js-line-number js-blob-rnum" data-line-number="17"></td>
          <td id="file-fluent-bit-ds-yaml-LC17" class="blob-code blob-code-inner js-file-line">        k8s-app: fluent-bit-logging</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L18" class="blob-num js-line-number js-blob-rnum" data-line-number="18"></td>
          <td id="file-fluent-bit-ds-yaml-LC18" class="blob-code blob-code-inner js-file-line">        version: v1</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L19" class="blob-num js-line-number js-blob-rnum" data-line-number="19"></td>
          <td id="file-fluent-bit-ds-yaml-LC19" class="blob-code blob-code-inner js-file-line">        kubernetes.io/cluster-service: &quot;true&quot;</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L20" class="blob-num js-line-number js-blob-rnum" data-line-number="20"></td>
          <td id="file-fluent-bit-ds-yaml-LC20" class="blob-code blob-code-inner js-file-line">      annotations:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L21" class="blob-num js-line-number js-blob-rnum" data-line-number="21"></td>
          <td id="file-fluent-bit-ds-yaml-LC21" class="blob-code blob-code-inner js-file-line">        prometheus.io/scrape: &quot;true&quot;</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L22" class="blob-num js-line-number js-blob-rnum" data-line-number="22"></td>
          <td id="file-fluent-bit-ds-yaml-LC22" class="blob-code blob-code-inner js-file-line">        prometheus.io/port: &quot;2020&quot;</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L23" class="blob-num js-line-number js-blob-rnum" data-line-number="23"></td>
          <td id="file-fluent-bit-ds-yaml-LC23" class="blob-code blob-code-inner js-file-line">        prometheus.io/path: /api/v1/metrics/prometheus</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L24" class="blob-num js-line-number js-blob-rnum" data-line-number="24"></td>
          <td id="file-fluent-bit-ds-yaml-LC24" class="blob-code blob-code-inner js-file-line">    spec:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L25" class="blob-num js-line-number js-blob-rnum" data-line-number="25"></td>
          <td id="file-fluent-bit-ds-yaml-LC25" class="blob-code blob-code-inner js-file-line">      containers:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L26" class="blob-num js-line-number js-blob-rnum" data-line-number="26"></td>
          <td id="file-fluent-bit-ds-yaml-LC26" class="blob-code blob-code-inner js-file-line">      &#8211; name: fluent-bit</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L27" class="blob-num js-line-number js-blob-rnum" data-line-number="27"></td>
          <td id="file-fluent-bit-ds-yaml-LC27" class="blob-code blob-code-inner js-file-line">        image: fluent/fluent-bit:1.3.11</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L28" class="blob-num js-line-number js-blob-rnum" data-line-number="28"></td>
          <td id="file-fluent-bit-ds-yaml-LC28" class="blob-code blob-code-inner js-file-line">        imagePullPolicy: Always</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L29" class="blob-num js-line-number js-blob-rnum" data-line-number="29"></td>
          <td id="file-fluent-bit-ds-yaml-LC29" class="blob-code blob-code-inner js-file-line">        ports:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L30" class="blob-num js-line-number js-blob-rnum" data-line-number="30"></td>
          <td id="file-fluent-bit-ds-yaml-LC30" class="blob-code blob-code-inner js-file-line">          &#8211; containerPort: 2020</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L31" class="blob-num js-line-number js-blob-rnum" data-line-number="31"></td>
          <td id="file-fluent-bit-ds-yaml-LC31" class="blob-code blob-code-inner js-file-line">        env:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L32" class="blob-num js-line-number js-blob-rnum" data-line-number="32"></td>
          <td id="file-fluent-bit-ds-yaml-LC32" class="blob-code blob-code-inner js-file-line">        &#8211; name: FLUENT_ELASTICSEARCH_HOST</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L33" class="blob-num js-line-number js-blob-rnum" data-line-number="33"></td>
          <td id="file-fluent-bit-ds-yaml-LC33" class="blob-code blob-code-inner js-file-line">          value: &quot;elasticsearch&quot;</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L34" class="blob-num js-line-number js-blob-rnum" data-line-number="34"></td>
          <td id="file-fluent-bit-ds-yaml-LC34" class="blob-code blob-code-inner js-file-line">        &#8211; name: FLUENT_ELASTICSEARCH_PORT</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L35" class="blob-num js-line-number js-blob-rnum" data-line-number="35"></td>
          <td id="file-fluent-bit-ds-yaml-LC35" class="blob-code blob-code-inner js-file-line">          value: &quot;9200&quot;</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L36" class="blob-num js-line-number js-blob-rnum" data-line-number="36"></td>
          <td id="file-fluent-bit-ds-yaml-LC36" class="blob-code blob-code-inner js-file-line">        &#8211; name: FLUENT_ELASTICSEARCH_USER</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L37" class="blob-num js-line-number js-blob-rnum" data-line-number="37"></td>
          <td id="file-fluent-bit-ds-yaml-LC37" class="blob-code blob-code-inner js-file-line">          value: &quot;elastic&quot;</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L38" class="blob-num js-line-number js-blob-rnum" data-line-number="38"></td>
          <td id="file-fluent-bit-ds-yaml-LC38" class="blob-code blob-code-inner js-file-line">        &#8211; name: FLUENT_ELASTICSEARCH_PASSWORD</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L39" class="blob-num js-line-number js-blob-rnum" data-line-number="39"></td>
          <td id="file-fluent-bit-ds-yaml-LC39" class="blob-code blob-code-inner js-file-line">          valueFrom:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L40" class="blob-num js-line-number js-blob-rnum" data-line-number="40"></td>
          <td id="file-fluent-bit-ds-yaml-LC40" class="blob-code blob-code-inner js-file-line">            secretKeyRef:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L41" class="blob-num js-line-number js-blob-rnum" data-line-number="41"></td>
          <td id="file-fluent-bit-ds-yaml-LC41" class="blob-code blob-code-inner js-file-line">              name: kibana-password</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L42" class="blob-num js-line-number js-blob-rnum" data-line-number="42"></td>
          <td id="file-fluent-bit-ds-yaml-LC42" class="blob-code blob-code-inner js-file-line">              key: password</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L43" class="blob-num js-line-number js-blob-rnum" data-line-number="43"></td>
          <td id="file-fluent-bit-ds-yaml-LC43" class="blob-code blob-code-inner js-file-line">        volumeMounts:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L44" class="blob-num js-line-number js-blob-rnum" data-line-number="44"></td>
          <td id="file-fluent-bit-ds-yaml-LC44" class="blob-code blob-code-inner js-file-line">        &#8211; name: varlog</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L45" class="blob-num js-line-number js-blob-rnum" data-line-number="45"></td>
          <td id="file-fluent-bit-ds-yaml-LC45" class="blob-code blob-code-inner js-file-line">          mountPath: /var/log</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L46" class="blob-num js-line-number js-blob-rnum" data-line-number="46"></td>
          <td id="file-fluent-bit-ds-yaml-LC46" class="blob-code blob-code-inner js-file-line">        &#8211; name: varlibdockercontainers</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L47" class="blob-num js-line-number js-blob-rnum" data-line-number="47"></td>
          <td id="file-fluent-bit-ds-yaml-LC47" class="blob-code blob-code-inner js-file-line">          mountPath: /var/lib/docker/containers</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L48" class="blob-num js-line-number js-blob-rnum" data-line-number="48"></td>
          <td id="file-fluent-bit-ds-yaml-LC48" class="blob-code blob-code-inner js-file-line">          readOnly: true</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L49" class="blob-num js-line-number js-blob-rnum" data-line-number="49"></td>
          <td id="file-fluent-bit-ds-yaml-LC49" class="blob-code blob-code-inner js-file-line">        &#8211; name: fluent-bit-config</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L50" class="blob-num js-line-number js-blob-rnum" data-line-number="50"></td>
          <td id="file-fluent-bit-ds-yaml-LC50" class="blob-code blob-code-inner js-file-line">          mountPath: /fluent-bit/etc/</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L51" class="blob-num js-line-number js-blob-rnum" data-line-number="51"></td>
          <td id="file-fluent-bit-ds-yaml-LC51" class="blob-code blob-code-inner js-file-line">      terminationGracePeriodSeconds: 10</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L52" class="blob-num js-line-number js-blob-rnum" data-line-number="52"></td>
          <td id="file-fluent-bit-ds-yaml-LC52" class="blob-code blob-code-inner js-file-line">      volumes:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L53" class="blob-num js-line-number js-blob-rnum" data-line-number="53"></td>
          <td id="file-fluent-bit-ds-yaml-LC53" class="blob-code blob-code-inner js-file-line">      &#8211; name: varlog</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L54" class="blob-num js-line-number js-blob-rnum" data-line-number="54"></td>
          <td id="file-fluent-bit-ds-yaml-LC54" class="blob-code blob-code-inner js-file-line">        hostPath:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L55" class="blob-num js-line-number js-blob-rnum" data-line-number="55"></td>
          <td id="file-fluent-bit-ds-yaml-LC55" class="blob-code blob-code-inner js-file-line">          path: /var/log</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L56" class="blob-num js-line-number js-blob-rnum" data-line-number="56"></td>
          <td id="file-fluent-bit-ds-yaml-LC56" class="blob-code blob-code-inner js-file-line">      &#8211; name: varlibdockercontainers</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L57" class="blob-num js-line-number js-blob-rnum" data-line-number="57"></td>
          <td id="file-fluent-bit-ds-yaml-LC57" class="blob-code blob-code-inner js-file-line">        hostPath:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L58" class="blob-num js-line-number js-blob-rnum" data-line-number="58"></td>
          <td id="file-fluent-bit-ds-yaml-LC58" class="blob-code blob-code-inner js-file-line">          path: /var/lib/docker/containers</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L59" class="blob-num js-line-number js-blob-rnum" data-line-number="59"></td>
          <td id="file-fluent-bit-ds-yaml-LC59" class="blob-code blob-code-inner js-file-line">      &#8211; name: fluent-bit-config</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L60" class="blob-num js-line-number js-blob-rnum" data-line-number="60"></td>
          <td id="file-fluent-bit-ds-yaml-LC60" class="blob-code blob-code-inner js-file-line">        configMap:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L61" class="blob-num js-line-number js-blob-rnum" data-line-number="61"></td>
          <td id="file-fluent-bit-ds-yaml-LC61" class="blob-code blob-code-inner js-file-line">          name: fluent-bit-config</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L62" class="blob-num js-line-number js-blob-rnum" data-line-number="62"></td>
          <td id="file-fluent-bit-ds-yaml-LC62" class="blob-code blob-code-inner js-file-line">      serviceAccountName: fluent-bit-sa</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L63" class="blob-num js-line-number js-blob-rnum" data-line-number="63"></td>
          <td id="file-fluent-bit-ds-yaml-LC63" class="blob-code blob-code-inner js-file-line">      tolerations:</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L64" class="blob-num js-line-number js-blob-rnum" data-line-number="64"></td>
          <td id="file-fluent-bit-ds-yaml-LC64" class="blob-code blob-code-inner js-file-line">      &#8211; key: node-role.kubernetes.io/master</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L65" class="blob-num js-line-number js-blob-rnum" data-line-number="65"></td>
          <td id="file-fluent-bit-ds-yaml-LC65" class="blob-code blob-code-inner js-file-line">        operator: Exists</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L66" class="blob-num js-line-number js-blob-rnum" data-line-number="66"></td>
          <td id="file-fluent-bit-ds-yaml-LC66" class="blob-code blob-code-inner js-file-line">        effect: NoSchedule</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L67" class="blob-num js-line-number js-blob-rnum" data-line-number="67"></td>
          <td id="file-fluent-bit-ds-yaml-LC67" class="blob-code blob-code-inner js-file-line">      &#8211; operator: &quot;Exists&quot;</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L68" class="blob-num js-line-number js-blob-rnum" data-line-number="68"></td>
          <td id="file-fluent-bit-ds-yaml-LC68" class="blob-code blob-code-inner js-file-line">        effect: &quot;NoExecute&quot;</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L69" class="blob-num js-line-number js-blob-rnum" data-line-number="69"></td>
          <td id="file-fluent-bit-ds-yaml-LC69" class="blob-code blob-code-inner js-file-line">      &#8211; operator: &quot;Exists&quot;</td>
        </tr>
        <tr>
          <td id="file-fluent-bit-ds-yaml-L70" class="blob-num js-line-number js-blob-rnum" data-line-number="70"></td>
          <td id="file-fluent-bit-ds-yaml-LC70" class="blob-code blob-code-inner js-file-line">        effect: &quot;NoSchedule&quot;</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/e2f65b3ba39930f1beebf17872d17e32/raw/b079ca55ce69d5ab7fd3198ca37ae782afca6efb/fluent-bit-ds.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/e2f65b3ba39930f1beebf17872d17e32#file-fluent-bit-ds-yaml" class="Link--inTextBlock">
          fluent-bit-ds.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<h5 class="wp-block-heading">Verify Fluent-bit Deployment</h5>



<p>By creating a pod that generates logs continuously, you can ensure that Fluent-bit is correctly collecting and forwarding the logs to Elasticsearch.</p>



<p class="has-text-align-justify">You can create a pod that generates logs continuously by using an image that has a script or a program that generates logs. For example, you can use the <code>busybox</code> image and run the <code>logger</code> command in a loop inside the pod.</p>



<p>Let’s create a test pod for Fluent-bit.</p>



<figure class="wp-block-embed is-type-rich is-provider-embed-handler wp-block-embed-embed-handler"><div class="wp-block-embed__wrapper">
<style>.gist table { margin-bottom: 0; }</style><div style="tab-size: 8" id="gist120371076" class="gist">
    <div class="gist-file" translate="no" data-color-mode="light" data-light-theme="light">
      <div class="gist-data">
        <div class="js-gist-file-update-container js-task-list-container">
  <div id="file-test-pod-yaml" class="file my-2">
    
    <div itemprop="text" class="Box-body p-0 blob-wrapper data type-yaml  ">

        
<div class="js-check-bidi js-blob-code-container blob-code-content">

  <template class="js-file-alert-template">
  <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
  <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
    <span>
      This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.
      <a class="Link--inTextBlock" href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a>
    </span>


  <div data-view-component="true" class="flash-action">        <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn">    Show hidden characters
</a>
</div>
</div></template>
<template class="js-line-alert-template">
  <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert">
    <path d="M6.457 1.047c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0 1 14.082 15H1.918a1.75 1.75 0 0 1-1.543-2.575Zm1.763.707a.25.25 0 0 0-.44 0L1.698 13.132a.25.25 0 0 0 .22.368h12.164a.25.25 0 0 0 .22-.368Zm.53 3.996v2.5a.75.75 0 0 1-1.5 0v-2.5a.75.75 0 0 1 1.5 0ZM9 11a1 1 0 1 1-2 0 1 1 0 0 1 2 0Z"></path>
</svg>
</span></template>

  <table data-hpc class="highlight tab-size js-file-line-container" data-tab-size="8" data-paste-markdown-skip data-tagsearch-path="test-pod.yaml">
        <tr>
          <td id="file-test-pod-yaml-L1" class="blob-num js-line-number js-blob-rnum" data-line-number="1"></td>
          <td id="file-test-pod-yaml-LC1" class="blob-code blob-code-inner js-file-line">apiVersion: v1</td>
        </tr>
        <tr>
          <td id="file-test-pod-yaml-L2" class="blob-num js-line-number js-blob-rnum" data-line-number="2"></td>
          <td id="file-test-pod-yaml-LC2" class="blob-code blob-code-inner js-file-line">kind: Pod</td>
        </tr>
        <tr>
          <td id="file-test-pod-yaml-L3" class="blob-num js-line-number js-blob-rnum" data-line-number="3"></td>
          <td id="file-test-pod-yaml-LC3" class="blob-code blob-code-inner js-file-line">metadata:</td>
        </tr>
        <tr>
          <td id="file-test-pod-yaml-L4" class="blob-num js-line-number js-blob-rnum" data-line-number="4"></td>
          <td id="file-test-pod-yaml-LC4" class="blob-code blob-code-inner js-file-line">  name: log-generator</td>
        </tr>
        <tr>
          <td id="file-test-pod-yaml-L5" class="blob-num js-line-number js-blob-rnum" data-line-number="5"></td>
          <td id="file-test-pod-yaml-LC5" class="blob-code blob-code-inner js-file-line">spec:</td>
        </tr>
        <tr>
          <td id="file-test-pod-yaml-L6" class="blob-num js-line-number js-blob-rnum" data-line-number="6"></td>
          <td id="file-test-pod-yaml-LC6" class="blob-code blob-code-inner js-file-line">  containers:</td>
        </tr>
        <tr>
          <td id="file-test-pod-yaml-L7" class="blob-num js-line-number js-blob-rnum" data-line-number="7"></td>
          <td id="file-test-pod-yaml-LC7" class="blob-code blob-code-inner js-file-line">  &#8211; name: log-generator</td>
        </tr>
        <tr>
          <td id="file-test-pod-yaml-L8" class="blob-num js-line-number js-blob-rnum" data-line-number="8"></td>
          <td id="file-test-pod-yaml-LC8" class="blob-code blob-code-inner js-file-line">    image: busybox</td>
        </tr>
        <tr>
          <td id="file-test-pod-yaml-L9" class="blob-num js-line-number js-blob-rnum" data-line-number="9"></td>
          <td id="file-test-pod-yaml-LC9" class="blob-code blob-code-inner js-file-line">    command: [&#39;sh&#39;, &#39;-c&#39;, &#39;i=0; while true; do echo &quot;$i: Log message&quot;; i=$((i+1)); sleep 1; done&#39;]</td>
        </tr>
  </table>
</div>


    </div>

  </div>
</div>

      </div>
      <div class="gist-meta">
        <a href="https://gist.github.com/Cpriyanshi77/60889ed3d66601e54df5236f326c733a/raw/a9f5d6e507dee224be072b965a0da72a09b60ae0/test-pod.yaml" style="float:right" class="Link--inTextBlock">view raw</a>
        <a href="https://gist.github.com/Cpriyanshi77/60889ed3d66601e54df5236f326c733a#file-test-pod-yaml" class="Link--inTextBlock">
          test-pod.yaml
        </a>
        hosted with &#10084; by <a class="Link--inTextBlock" href="https://github.com">GitHub</a>
      </div>
    </div>
</div>

</div></figure>



<p></p>



<p class="has-text-align-justify">By doing this, you are able to verify that Fluent-bit is working as expected, and it is collecting and forwarding the logs to Elasticsearch, where they can be analyzed and visualized in Kibana.</p>



<p>That’s it!</p>



<p>Hope you found this tutorial helpful, And that you will be able to set up the EFK stack for logging in Kubernetes with ease.</p>



<p><strong>Blog Pundits: <a rel="noreferrer noopener" href="https://opstree.com/blog//author/bhupendersinghb5dca0b393/" target="_blank">Bhupender Rawat</a> and <a rel="noreferrer noopener" href="https://opstree.com/blog//author/sandeep7c51ad81ba/" target="_blank">Sandeep Rawat</a></strong></p>



<p><strong><a href="https://opstree.com/contact-us/?utm_source=WordPress&amp;utm_medium=Blog&amp;utm_campaign=Protected+EFK+Stack+Setup+for+Kubernetes" target="_blank" rel="noreferrer noopener">OpsTree</a> is an End-to-End DevOps Solution Provider.</strong></p>



<div class="wp-block-buttons is-layout-flex wp-block-buttons-is-layout-flex">
<div class="wp-block-button"><a class="wp-block-button__link wp-element-button" href="https://opstree.com/contact-us/?utm_source=WordPress&amp;utm_medium=Blog&amp;utm_campaign=Protected+EFK+Stack+Setup+for+Kubernetes" target="_blank" rel="noreferrer noopener">Contact Us</a></div>
</div>



<p class="has-text-align-center"><strong>Connect with Us</strong></p>



<ul class="wp-block-social-links aligncenter is-content-justification-center is-layout-flex wp-container-core-social-links-is-layout-1 wp-block-social-links-is-layout-flex"><li class="wp-social-link wp-social-link-linkedin  wp-block-social-link"><a href="https://www.linkedin.com/company/opstree-solutions" class="wp-block-social-link-anchor"><svg width="24" height="24" viewBox="0 0 24 24" version="1.1" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false"><path d="M19.7,3H4.3C3.582,3,3,3.582,3,4.3v15.4C3,20.418,3.582,21,4.3,21h15.4c0.718,0,1.3-0.582,1.3-1.3V4.3 C21,3.582,20.418,3,19.7,3z M8.339,18.338H5.667v-8.59h2.672V18.338z M7.004,8.574c-0.857,0-1.549-0.694-1.549-1.548 c0-0.855,0.691-1.548,1.549-1.548c0.854,0,1.547,0.694,1.547,1.548C8.551,7.881,7.858,8.574,7.004,8.574z M18.339,18.338h-2.669 v-4.177c0-0.996-0.017-2.278-1.387-2.278c-1.389,0-1.601,1.086-1.601,2.206v4.249h-2.667v-8.59h2.559v1.174h0.037 c0.356-0.675,1.227-1.387,2.526-1.387c2.703,0,3.203,1.779,3.203,4.092V18.338z"></path></svg><span class="wp-block-social-link-label screen-reader-text">LinkedIn</span></a></li>

<li class="wp-social-link wp-social-link-youtube  wp-block-social-link"><a href="https://www.youtube.com/channel/UCeLma6SpNYH7jjYKSBNSexw" class="wp-block-social-link-anchor"><svg width="24" height="24" viewBox="0 0 24 24" version="1.1" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false"><path d="M21.8,8.001c0,0-0.195-1.378-0.795-1.985c-0.76-0.797-1.613-0.801-2.004-0.847c-2.799-0.202-6.997-0.202-6.997-0.202 h-0.009c0,0-4.198,0-6.997,0.202C4.608,5.216,3.756,5.22,2.995,6.016C2.395,6.623,2.2,8.001,2.2,8.001S2,9.62,2,11.238v1.517 c0,1.618,0.2,3.237,0.2,3.237s0.195,1.378,0.795,1.985c0.761,0.797,1.76,0.771,2.205,0.855c1.6,0.153,6.8,0.201,6.8,0.201 s4.203-0.006,7.001-0.209c0.391-0.047,1.243-0.051,2.004-0.847c0.6-0.607,0.795-1.985,0.795-1.985s0.2-1.618,0.2-3.237v-1.517 C22,9.62,21.8,8.001,21.8,8.001z M9.935,14.594l-0.001-5.62l5.404,2.82L9.935,14.594z"></path></svg><span class="wp-block-social-link-label screen-reader-text">YouTube</span></a></li>

<li class="wp-social-link wp-social-link-github  wp-block-social-link"><a href="https://github.com/OpsTree" class="wp-block-social-link-anchor"><svg width="24" height="24" viewBox="0 0 24 24" version="1.1" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false"><path d="M12,2C6.477,2,2,6.477,2,12c0,4.419,2.865,8.166,6.839,9.489c0.5,0.09,0.682-0.218,0.682-0.484 c0-0.236-0.009-0.866-0.014-1.699c-2.782,0.602-3.369-1.34-3.369-1.34c-0.455-1.157-1.11-1.465-1.11-1.465 c-0.909-0.62,0.069-0.608,0.069-0.608c1.004,0.071,1.532,1.03,1.532,1.03c0.891,1.529,2.341,1.089,2.91,0.833 c0.091-0.647,0.349-1.086,0.635-1.337c-2.22-0.251-4.555-1.111-4.555-4.943c0-1.091,0.39-1.984,1.03-2.682 C6.546,8.54,6.202,7.524,6.746,6.148c0,0,0.84-0.269,2.75,1.025C10.295,6.95,11.15,6.84,12,6.836 c0.85,0.004,1.705,0.114,2.504,0.336c1.909-1.294,2.748-1.025,2.748-1.025c0.546,1.376,0.202,2.394,0.1,2.646 c0.64,0.699,1.026,1.591,1.026,2.682c0,3.841-2.337,4.687-4.565,4.935c0.359,0.307,0.679,0.917,0.679,1.852 c0,1.335-0.012,2.415-0.012,2.741c0,0.269,0.18,0.579,0.688,0.481C19.138,20.161,22,16.416,22,12C22,6.477,17.523,2,12,2z"></path></svg><span class="wp-block-social-link-label screen-reader-text">GitHub</span></a></li>

<li class="wp-social-link wp-social-link-facebook  wp-block-social-link"><a href="https://www.facebook.com/opstree" class="wp-block-social-link-anchor"><svg width="24" height="24" viewBox="0 0 24 24" version="1.1" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false"><path d="M12 2C6.5 2 2 6.5 2 12c0 5 3.7 9.1 8.4 9.9v-7H7.9V12h2.5V9.8c0-2.5 1.5-3.9 3.8-3.9 1.1 0 2.2.2 2.2.2v2.5h-1.3c-1.2 0-1.6.8-1.6 1.6V12h2.8l-.4 2.9h-2.3v7C18.3 21.1 22 17 22 12c0-5.5-4.5-10-10-10z"></path></svg><span class="wp-block-social-link-label screen-reader-text">Facebook</span></a></li>

<li class="wp-social-link wp-social-link-medium  wp-block-social-link"><a href="https://medium.com/buildpiper" class="wp-block-social-link-anchor"><svg width="24" height="24" viewBox="0 0 24 24" version="1.1" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false"><path d="M20.962,7.257l-5.457,8.867l-3.923-6.375l3.126-5.08c0.112-0.182,0.319-0.286,0.527-0.286c0.05,0,0.1,0.008,0.149,0.02 c0.039,0.01,0.078,0.023,0.114,0.041l5.43,2.715l0.006,0.003c0.004,0.002,0.007,0.006,0.011,0.008 C20.971,7.191,20.98,7.227,20.962,7.257z M9.86,8.592v5.783l5.14,2.57L9.86,8.592z M15.772,17.331l4.231,2.115 C20.554,19.721,21,19.529,21,19.016V8.835L15.772,17.331z M8.968,7.178L3.665,4.527C3.569,4.479,3.478,4.456,3.395,4.456 C3.163,4.456,3,4.636,3,4.938v11.45c0,0.306,0.224,0.669,0.498,0.806l4.671,2.335c0.12,0.06,0.234,0.088,0.337,0.088 c0.29,0,0.494-0.225,0.494-0.602V7.231C9,7.208,8.988,7.188,8.968,7.178z"></path></svg><span class="wp-block-social-link-label screen-reader-text">Medium</span></a></li></ul>
<div class="sharedaddy sd-sharing-enabled"><div class="robots-nocontent sd-block sd-social sd-social-icon-text sd-sharing"><h3 class="sd-title">Share this:</h3><div class="sd-content"><ul><li class="share-twitter"><a rel="nofollow noopener noreferrer" data-shared="sharing-twitter-12726" class="share-twitter sd-button share-icon" href="https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/?share=twitter" target="_blank" title="Click to share on Twitter" ><span>Twitter</span></a></li><li class="share-linkedin"><a rel="nofollow noopener noreferrer" data-shared="sharing-linkedin-12726" class="share-linkedin sd-button share-icon" href="https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/?share=linkedin" target="_blank" title="Click to share on LinkedIn" ><span>LinkedIn</span></a></li><li class="share-facebook"><a rel="nofollow noopener noreferrer" data-shared="sharing-facebook-12726" class="share-facebook sd-button share-icon" href="https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/?share=facebook" target="_blank" title="Click to share on Facebook" ><span>Facebook</span></a></li><li class="share-end"></li></ul></div></div></div><div class='sharedaddy sd-block sd-like jetpack-likes-widget-wrapper jetpack-likes-widget-unloaded' id='like-post-wrapper-231085182-12726-6771220e5cbbd' data-src='https://widgets.wp.com/likes/?ver=13.3.2#blog_id=231085182&amp;post_id=12726&amp;origin=opstree.com&amp;obj_id=231085182-12726-6771220e5cbbd&amp;n=1' data-name='like-post-frame-231085182-12726-6771220e5cbbd' data-title='Like or Reblog'><h3 class="sd-title">Like this:</h3><div class='likes-widget-placeholder post-likes-widget-placeholder' style='height: 55px;'><span class='button'><span>Like</span></span> <span class="loading">Loading...</span></div><span class='sd-text-color'></span><a class='sd-link-color'></a></div>
<div id='jp-relatedposts' class='jp-relatedposts' >
	<h3 class="jp-relatedposts-headline"><em>Related</em></h3>
</div>
<div class="author-info">
	<div class="author-avatar">
		<img alt='' src='https://secure.gravatar.com/avatar/863b5758f40b7e44062b3a48738d7d12?s=42&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/863b5758f40b7e44062b3a48738d7d12?s=84&#038;d=identicon&#038;r=g 2x' class='avatar avatar-42 photo' height='42' width='42' decoding='async'/>	</div><!-- .author-avatar -->

	<div class="author-description">
		<h2 class="author-title"><span class="author-heading">Author:</span> Priyanshi Chauhan</h2>

		<p class="author-bio">
			I am a DevOps Engineer			<a class="author-link" href="https://opstree.com/blog/author/priyanshi0707/" rel="author">
				View all posts by Priyanshi Chauhan			</a>
		</p><!-- .author-bio -->
	</div><!-- .author-description -->
</div><!-- .author-info -->
	</div><!-- .entry-content -->

	<footer class="entry-footer">
		<span class="byline"><span class="author vcard"><img alt='' src='https://secure.gravatar.com/avatar/863b5758f40b7e44062b3a48738d7d12?s=49&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/863b5758f40b7e44062b3a48738d7d12?s=98&#038;d=identicon&#038;r=g 2x' class='avatar avatar-49 photo' height='49' width='49' decoding='async'/><span class="screen-reader-text">Author </span> <a class="url fn n" href="https://opstree.com/blog/author/priyanshi0707/">Priyanshi Chauhan</a></span></span><span class="posted-on"><span class="screen-reader-text">Posted on </span><a href="https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/" rel="bookmark"><time class="entry-date published" datetime="2023-01-24T11:53:29+05:30">January 24, 2023</time><time class="updated" datetime="2023-08-21T11:25:58+05:30">August 21, 2023</time></a></span><span class="cat-links"><span class="screen-reader-text">Categories </span><a href="https://opstree.com/blog/category/devops/" rel="category tag">DevOps</a></span><span class="tags-links"><span class="screen-reader-text">Tags </span><a href="https://opstree.com/blog/tag/automation/" rel="tag">Automation</a>, <a href="https://opstree.com/blog/tag/devops/" rel="tag">DevOps</a>, <a href="https://opstree.com/blog/tag/devops-solutioning/" rel="tag">Devops Solutioning</a>, <a href="https://opstree.com/blog/tag/efk/" rel="tag">EFK</a>, <a href="https://opstree.com/blog/tag/kubernetes/" rel="tag">Kubernetes</a>, <a href="https://opstree.com/blog/tag/monitoring/" rel="tag">Monitoring</a>, <a href="https://opstree.com/blog/tag/technical-blogs/" rel="tag">technical blogs</a></span>			</footer><!-- .entry-footer -->
</article><!-- #post-12726 -->

<div id="comments" class="comments-area">

			<h2 class="comments-title">
			2 thoughts on &ldquo;Protected EFK Stack Setup for Kubernetes&rdquo;		</h2>

		
		<ol class="comment-list">
					<li id="comment-10457" class="comment even thread-even depth-1">
			<article id="div-comment-10457" class="comment-body">
				<footer class="comment-meta">
					<div class="comment-author vcard">
						<img alt='' src='https://secure.gravatar.com/avatar/74ec7b8287279a6270583a118cd955bb?s=42&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/74ec7b8287279a6270583a118cd955bb?s=84&#038;d=identicon&#038;r=g 2x' class='avatar avatar-42 photo' height='42' width='42' decoding='async'/>						<b class="fn">Chadley Wilson</b> <span class="says">says:</span>					</div><!-- .comment-author -->

					<div class="comment-metadata">
						<a href="https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/#comment-10457"><time datetime="2023-05-11T18:11:42+05:30">May 11, 2023 at 6:11 pm</time></a>					</div><!-- .comment-metadata -->

									</footer><!-- .comment-meta -->

				<div class="comment-content">
					<p>I don&#8217;t think this guide works any more<br />
for one you have use tokens now, the password is longer accepted. and secondly the pv clain doesn&#8217;t work you have the first create a local  hostpath or a block PV.<br />
Too many things to fix here to follow the guide.</p>
<div class='jetpack-comment-likes-widget-wrapper jetpack-likes-widget-unloaded' id='like-comment-wrapper-231085182-10457-6771220e5f8ac' data-src='https://widgets.wp.com/likes/#blog_id=231085182&amp;comment_id=10457&amp;origin=opstree.com&amp;obj_id=231085182-10457-6771220e5f8ac' data-name='like-comment-frame-231085182-10457-6771220e5f8ac'>
<div class='likes-widget-placeholder comment-likes-widget-placeholder comment-likes'><span class='loading'>Loading...</span></div>
<div class='comment-likes-widget jetpack-likes-widget comment-likes'><span class='comment-like-feedback'></span><span class='sd-text-color'></span><a class='sd-link-color'></a></div>
</div>
				</div><!-- .comment-content -->

				<div class="reply"><a rel='nofollow' class='comment-reply-link' href='#comment-10457' data-commentid="10457" data-postid="12726" data-belowelement="div-comment-10457" data-respondelement="respond" data-replyto="Reply to Chadley Wilson" aria-label='Reply to Chadley Wilson'>Reply</a></div>			</article><!-- .comment-body -->
		</li><!-- #comment-## -->
		<li id="comment-10838" class="comment odd alt thread-odd thread-alt depth-1">
			<article id="div-comment-10838" class="comment-body">
				<footer class="comment-meta">
					<div class="comment-author vcard">
						<img alt='' src='https://secure.gravatar.com/avatar/dc5fdfa8616442612858a03b339b64a4?s=42&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/dc5fdfa8616442612858a03b339b64a4?s=84&#038;d=identicon&#038;r=g 2x' class='avatar avatar-42 photo' height='42' width='42' loading='lazy' decoding='async'/>						<b class="fn">Gary B.</b> <span class="says">says:</span>					</div><!-- .comment-author -->

					<div class="comment-metadata">
						<a href="https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/#comment-10838"><time datetime="2023-06-15T22:51:15+05:30">June 15, 2023 at 10:51 pm</time></a>					</div><!-- .comment-metadata -->

									</footer><!-- .comment-meta -->

				<div class="comment-content">
					<p>Could be nice but incomplete, too many things to fix beginning from dashes, big K in kubectl, missing apply and port-forward commands and a namespace&#8230; But in the end it you just can&#8217;t login with set credentials</p>
<div class='jetpack-comment-likes-widget-wrapper jetpack-likes-widget-unloaded' id='like-comment-wrapper-231085182-10838-6771220e5fd27' data-src='https://widgets.wp.com/likes/#blog_id=231085182&amp;comment_id=10838&amp;origin=opstree.com&amp;obj_id=231085182-10838-6771220e5fd27' data-name='like-comment-frame-231085182-10838-6771220e5fd27'>
<div class='likes-widget-placeholder comment-likes-widget-placeholder comment-likes'><span class='loading'>Loading...</span></div>
<div class='comment-likes-widget jetpack-likes-widget comment-likes'><span class='comment-like-feedback'></span><span class='sd-text-color'></span><a class='sd-link-color'></a></div>
</div>
				</div><!-- .comment-content -->

				<div class="reply"><a rel='nofollow' class='comment-reply-link' href='#comment-10838' data-commentid="10838" data-postid="12726" data-belowelement="div-comment-10838" data-respondelement="respond" data-replyto="Reply to Gary B." aria-label='Reply to Gary B.'>Reply</a></div>			</article><!-- .comment-body -->
		</li><!-- #comment-## -->
		</ol><!-- .comment-list -->

		
	
	
	
		<div id="respond" class="comment-respond">
			<h3 id="reply-title" class="comment-reply-title">Leave a Reply<small><a rel="nofollow" id="cancel-comment-reply-link" href="/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/#respond" style="display:none;">Cancel reply</a></small></h3>			<form id="commentform" class="comment-form">
				<iframe
					title="Comment Form"
					src="https://jetpack.wordpress.com/jetpack-comment/?blogid=231085182&#038;postid=12726&#038;comment_registration=0&#038;require_name_email=1&#038;stc_enabled=1&#038;stb_enabled=1&#038;show_avatars=1&#038;avatar_default=identicon&#038;greeting=Leave+a+Reply&#038;jetpack_comments_nonce=cfea330f83&#038;greeting_reply=Leave+a+Reply+to+%25s&#038;color_scheme=light&#038;lang=en_US&#038;jetpack_version=13.3.2&#038;show_cookie_consent=10&#038;has_cookie_consent=0&#038;is_current_user_subscribed=0&#038;token_key=%3Bnormal%3B&#038;sig=dc8af356871e7567d98b04e8f495e6270b1c35a2#parent=https%3A%2F%2Fopstree.com%2Fblog%2F2023%2F01%2F24%2Fprotected-efk-stack-setup-for-kubernetes%2F"
											name="jetpack_remote_comment"
						style="width:100%; height: 430px; border:0;"
										class="jetpack_remote_comment"
					id="jetpack_remote_comment"
					sandbox="allow-same-origin allow-top-navigation allow-scripts allow-forms allow-popups"
				>
									</iframe>
									<!--[if !IE]><!-->
					<script>
						document.addEventListener('DOMContentLoaded', function () {
							var commentForms = document.getElementsByClassName('jetpack_remote_comment');
							for (var i = 0; i < commentForms.length; i++) {
								commentForms[i].allowTransparency = false;
								commentForms[i].scrolling = 'no';
							}
						});
					</script>
					<!--<![endif]-->
							</form>
		</div>

		
		<input type="hidden" name="comment_parent" id="comment_parent" value="" />

		
</div><!-- .comments-area -->

	<nav class="navigation post-navigation" aria-label="Posts">
		<h2 class="screen-reader-text">Post navigation</h2>
		<div class="nav-links"><div class="nav-previous"><a href="https://opstree.com/blog/2023/01/17/on-premise-setup-of-kubernetes-cluster-components-offline-mode-part-2/" rel="prev"><span class="meta-nav" aria-hidden="true">Previous</span> <span class="screen-reader-text">Previous post:</span> <span class="post-title">On-Premise Setup of Kubernetes Cluster Components (Offline Mode) &#8211; PART 2</span></a></div><div class="nav-next"><a href="https://opstree.com/blog/2023/01/31/kubernetes-cri-container-runtime-interface/" rel="next"><span class="meta-nav" aria-hidden="true">Next</span> <span class="screen-reader-text">Next post:</span> <span class="post-title">Kubernetes CRI — Container Runtime Interface</span></a></div></div>
	</nav>
	</main><!-- .site-main -->

	
</div><!-- .content-area -->


	<aside id="secondary" class="sidebar widget-area">
		<section id="search-1" class="widget widget_search">
<form role="search" method="get" class="search-form" action="https://opstree.com/blog/">
	<label>
		<span class="screen-reader-text">
			Search for:		</span>
		<input type="search" class="search-field" placeholder="Search &hellip;" value="" name="s" />
	</label>
	<button type="submit" class="search-submit"><span class="screen-reader-text">
		Search	</span></button>
</form>
</section><section id="block-31" class="widget widget_block widget_media_image">
<figure class="wp-block-image size-full has-lightbox"><a href="https://opstree.com/ebooks/ebook-ultimate-guide-to-delivering-end-to-end-data-strategy/" target="_blank" rel=" noreferrer noopener"><img loading="lazy" decoding="async" width="576" height="900" src="https://opstree.com/blog/wp-content/uploads/2024/11/End-to-End-Data-Strategy.jpg" alt="" class="wp-image-19431" srcset="https://i0.wp.com/opstree.com/blog/wp-content/uploads/2024/11/End-to-End-Data-Strategy.jpg?w=576&amp;ssl=1 576w, https://i0.wp.com/opstree.com/blog/wp-content/uploads/2024/11/End-to-End-Data-Strategy.jpg?resize=192%2C300&amp;ssl=1 192w" sizes="(max-width: 576px) 85vw, 576px" /></a></figure>
</section><section id="authors-6" class="widget widget_authors"><h2 class="widget-title">Author</h2><ul><li><a href="https://opstree.com/blog/author/abhishekbhardwaj510/"> <img alt='1' src='https://secure.gravatar.com/avatar/caac723e8296d6ed94200f570007c5e0?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/caac723e8296d6ed94200f570007c5e0?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Abhishek Dubey</strong></a><ul><li><a href="https://opstree.com/blog/2024/07/01/integration-of-prometheus-with-cortex/" title="Integration of Prometheus with Cortex">Integration of Prometheus with Cortex</a></li><li><a href="https://opstree.com/blog/2024/02/06/cassandra-to-scylladb-migration-without-any-downtime/" title="Cassandra to ScyllaDB Migration Without Any Downtime">Cassandra to ScyllaDB Migration Without Any Downtime</a></li><li><a href="https://opstree.com/blog/2023/07/18/continuation-of-redis-throughput-and-management/" title="Continuation Of Redis Throughput and Management">Continuation Of Redis Throughput and Management</a></li><li><a href="https://opstree.com/blog/2022/12/27/all-redis-setup-under-7-minutes/" title="All Redis Setup Under 7 Minutes!">All Redis Setup Under 7 Minutes!</a></li><li><a href="https://opstree.com/blog/2022/12/06/kubernetes-csi-container-storage-interface-part-1/" title="Kubernetes CSI: Container Storage Interface &#8211; Part 1">Kubernetes CSI: Container Storage Interface &#8211; Part 1</a></li></ul></li><li><a href="https://opstree.com/blog/author/abhishekoo4/"> <img alt='1' src='https://secure.gravatar.com/avatar/9162389b77fa83266a23a0cfcbb92f42?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/9162389b77fa83266a23a0cfcbb92f42?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Abhishek Vishwakarma</strong></a><ul><li><a href="https://opstree.com/blog/2021/10/05/percona-postgresql-through-awsesomeosm-ansible-role/" title="Setup Percona Postgresql Through the Awsesome(OSM) Ansible Role">Setup Percona Postgresql Through the Awsesome(OSM) Ansible Role</a></li></ul></li><li><a href="https://opstree.com/blog/author/acakashi/"> <img alt='1' src='https://secure.gravatar.com/avatar/10d5fdde476e8c28d8a795f61ad83860?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/10d5fdde476e8c28d8a795f61ad83860?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Akash Sharma</strong></a><ul><li><a href="https://opstree.com/blog/2023/07/04/prtg-network-monitor-why-and-how/" title="PRTG Network Monitor: Why and How?">PRTG Network Monitor: Why and How?</a></li></ul></li><li><a href="https://opstree.com/blog/author/adeel109/"> <img alt='1' src='https://secure.gravatar.com/avatar/8c1208ea43dbf283bfaac6f98f7e6e85?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/8c1208ea43dbf283bfaac6f98f7e6e85?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Adeel</strong></a><ul><li><a href="https://opstree.com/blog/2022/02/22/handling-private-affair-a-guide-to-secrets-management-system/" title="Handling Private Affair: A Guide to Secrets Management System">Handling Private Affair: A Guide to Secrets Management System</a></li><li><a href="https://opstree.com/blog/2021/02/23/how-dhcp-and-dns-are-managed-in-amazon-vpc/" title="How DHCP and DNS are managed in Amazon VPC">How DHCP and DNS are managed in Amazon VPC</a></li><li><a href="https://opstree.com/blog/2020/08/18/haproxy-hurdles-walkthrough/" title="HAProxy Hurdles Walkthrough">HAProxy Hurdles Walkthrough</a></li><li><a href="https://opstree.com/blog/2020/06/09/make-your-own-rules-elastalert-style/" title="Make Your Own Rules, ElastAlert Style">Make Your Own Rules, ElastAlert Style</a></li><li><a href="https://opstree.com/blog/2020/04/01/stash-logs-better-with-logstash/" title="Stash logs better with Logstash">Stash logs better with Logstash</a></li></ul></li><li><a href="https://opstree.com/blog/author/adityakaushik1005/"> <img alt='1' src='https://secure.gravatar.com/avatar/141b41fb538f1b2041a516386ab36228?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/141b41fb538f1b2041a516386ab36228?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>adityakaushik1005</strong></a><ul><li><a href="https://opstree.com/blog/2022/07/12/introduction-to-siege/" title="Introduction to Siege">Introduction to Siege</a></li></ul></li><li><a href="https://opstree.com/blog/author/akankshasrivastva/"> <img alt='1' src='https://secure.gravatar.com/avatar/0acf3f2dace713d126b3d7635990fa05?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/0acf3f2dace713d126b3d7635990fa05?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>akankshasrivastva</strong></a><ul><li><a href="https://opstree.com/blog/2023/03/07/servicenow-azure-devops-integration/" title="ServiceNow &#8211; Azure DevOps Integration">ServiceNow &#8211; Azure DevOps Integration</a></li><li><a href="https://opstree.com/blog/2021/08/04/the-migration-of-postgresql-using-azure-dms/" title="The Migration of Postgresql using Azure DMS">The Migration of Postgresql using Azure DMS</a></li></ul></li><li><a href="https://opstree.com/blog/author/akashparashartalviewcom/"> <img alt='1' src='https://secure.gravatar.com/avatar/7ac624c1bf1a4070b77e33adcce7d94d?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/7ac624c1bf1a4070b77e33adcce7d94d?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>akashparashartalviewcom</strong></a><ul><li><a href="https://opstree.com/blog/2022/05/03/praeco-alerting-for-elasticsearch-part-1/" title="Praeco Alerting for ElasticSearch (Part -1)">Praeco Alerting for ElasticSearch (Part -1)</a></li></ul></li><li><a href="https://opstree.com/blog/author/amitkumar150/"> <img alt='1' src='https://secure.gravatar.com/avatar/3b4335a1c10d80dc0b055ffa9efec2b8?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/3b4335a1c10d80dc0b055ffa9efec2b8?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>AmitKumar</strong></a><ul><li><a href="https://opstree.com/blog/2020/09/08/why-apm-is-extremely-significant/" title="Why APM is Extremely Significant!">Why APM is Extremely Significant!</a></li></ul></li><li><a href="https://opstree.com/blog/author/anjali152/"> <img alt='1' src='https://secure.gravatar.com/avatar/ff33b9331a3f363407ac324e9f27ae49?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/ff33b9331a3f363407ac324e9f27ae49?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>anjalisingh</strong></a><ul><li><a href="https://opstree.com/blog/2021/03/26/analyzing-latest-whatsapp-scam-leaking-s3-bucket/" title="Analyzing Latest WhatsApp Scam Leaking S3 Bucket">Analyzing Latest WhatsApp Scam Leaking S3 Bucket</a></li><li><a href="https://opstree.com/blog/2020/07/21/out-of-band-rce-ctf-walkthrough/" title="Out-Of-Band RCE: CTF Walkthrough">Out-Of-Band RCE: CTF Walkthrough</a></li><li><a href="https://opstree.com/blog/2020/04/29/linux-os-hardening-cis-benchmarks/" title="Linux OS Hardening: CIS Benchmarks">Linux OS Hardening: CIS Benchmarks</a></li></ul></li><li><a href="https://opstree.com/blog/author/anjali_kaushal/"> <img alt='1' src='https://secure.gravatar.com/avatar/7c1b313fbc64fd0d9cee72ae144f4f54?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/7c1b313fbc64fd0d9cee72ae144f4f54?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Anjali Kaushal</strong></a><ul><li><a href="https://opstree.com/blog/2024/06/04/harnessing-the-power-of-lokis-json-log-parsing-in-grafana/" title="Harnessing the Power of Loki&#8217;s JSON Log Parsing in Grafana">Harnessing the Power of Loki&#8217;s JSON Log Parsing in Grafana</a></li></ul></li><li><a href="https://opstree.com/blog/author/ankitot100/"> <img alt='1' src='https://secure.gravatar.com/avatar/6b9e080fbd8c889e2b08508fb409d3b9?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/6b9e080fbd8c889e2b08508fb409d3b9?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>ankitot100</strong></a><ul><li><a href="https://opstree.com/blog/2019/05/21/lets-get-started-with-packer/" title="Lets Get Started With Packer">Lets Get Started With Packer</a></li><li><a href="https://opstree.com/blog/2019/05/21/intro-to-packer/" title="Intro to Packer">Intro to Packer</a></li></ul></li><li><a href="https://opstree.com/blog/author/ankitrathi03/"> <img alt='1' src='https://secure.gravatar.com/avatar/7a1a23af94f0c8ba09065eecb29d9ee5?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/7a1a23af94f0c8ba09065eecb29d9ee5?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ankit Rathi</strong></a><ul><li><a href="https://opstree.com/blog/2023/12/05/ecs-capacity-provider-strategy/" title="ECS | Capacity Provider Strategy">ECS | Capacity Provider Strategy</a></li><li><a href="https://opstree.com/blog/2023/09/19/applications-hosting-on-ecs/" title="Applications Hosting  on ECS">Applications Hosting  on ECS</a></li><li><a href="https://opstree.com/blog/2023/07/25/multi-account-management-using-aws-control-tower/" title="Multi-Account Management using AWS Control Tower">Multi-Account Management using AWS Control Tower</a></li></ul></li><li><a href="https://opstree.com/blog/author/ankurvaishopstreecom/"> <img alt='1' src='https://secure.gravatar.com/avatar/c729bf9ed69bf6f17a32942bcf9acb84?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/c729bf9ed69bf6f17a32942bcf9acb84?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ankur Vaish</strong></a><ul><li><a href="https://opstree.com/blog/2022/11/01/kafka-within-efk-monitoring/" title="Kafka within EFK Monitoring">Kafka within EFK Monitoring</a></li></ul></li><li><a href="https://opstree.com/blog/author/ankurvermaed60be2ca9/"> <img alt='1' src='https://secure.gravatar.com/avatar/6057d7bd4387695e2d3073bbc923ab61?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/6057d7bd4387695e2d3073bbc923ab61?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ankur Verma</strong></a><ul><li><a href="https://opstree.com/blog/2021/04/06/optimize-java-8-with-docker/" title="Optimize Java 8 with Docker">Optimize Java 8 with Docker</a></li></ul></li><li><a href="https://opstree.com/blog/author/anshul-kichara/"> <img alt='1' src='https://secure.gravatar.com/avatar/da2c9138c866fcf7ddd3ae3ce2443fd9?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/da2c9138c866fcf7ddd3ae3ce2443fd9?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Anshul Kichara</strong></a><ul><li><a href="https://opstree.com/blog/2024/12/20/how-to-create-a-sitemap-for-a-website/" title="How to Create a Sitemap for a Website">How to Create a Sitemap for a Website</a></li></ul></li><li><a href="https://opstree.com/blog/author/antrasrivastava12/"> <img alt='1' src='https://secure.gravatar.com/avatar/b9e6dd34c46e0d6b089ec7ca8791ef2e?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/b9e6dd34c46e0d6b089ec7ca8791ef2e?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Antra Srivastava</strong></a><ul><li><a href="https://opstree.com/blog/2023/04/25/introduction-to-azure-iot-central/" title="Introduction to Azure IoT Central">Introduction to Azure IoT Central</a></li></ul></li><li><a href="https://opstree.com/blog/author/arpeetgupta/"> <img alt='1' src='https://secure.gravatar.com/avatar/2908543bd99fc328b6d529699e26a278?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/2908543bd99fc328b6d529699e26a278?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Arpeet Gupta</strong></a><ul><li><a href="https://opstree.com/blog/2020/10/06/elasticsearch-cluster-monitoring/" title="Elasticsearch Cluster Monitoring">Elasticsearch Cluster Monitoring</a></li><li><a href="https://opstree.com/blog/2020/08/11/elasticsearch-garbage-collector-frequent-execution-issue/" title="Elasticsearch Garbage Collector Frequent Execution Issue">Elasticsearch Garbage Collector Frequent Execution Issue</a></li><li><a href="https://opstree.com/blog/2020/06/30/cache-using-cloudflare-workers-cache-api/" title="Cache Using Cloudflare Workers&#8217; Cache API">Cache Using Cloudflare Workers&#8217; Cache API</a></li><li><a href="https://opstree.com/blog/2020/05/27/ip-whitelisting-using-istio-policy-on-kubernetes-microservices/" title="IP Whitelisting Using Istio Policy On Kubernetes Microservices">IP Whitelisting Using Istio Policy On Kubernetes Microservices</a></li><li><a href="https://opstree.com/blog/2020/04/07/preserve-source-ip-in-aws-classic-load-balancer-and-istios-envoy-using-proxy-protocol/" title="Preserve Source IP In AWS Classic Load-Balancer And Istio&#8217;s Envoy Using Proxy Protocol">Preserve Source IP In AWS Classic Load-Balancer And Istio&#8217;s Envoy Using Proxy Protocol</a></li></ul></li><li><a href="https://opstree.com/blog/author/asawatharsh/"> <img alt='1' src='https://secure.gravatar.com/avatar/99448473581227daad79c944d962c803?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/99448473581227daad79c944d962c803?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>harshvardhan</strong></a><ul><li><a href="https://opstree.com/blog/2019/11/26/one-more-reason-to-use-docker-part-ii/" title="One more reason to use Docker &#8211; part II">One more reason to use Docker &#8211; part II</a></li><li><a href="https://opstree.com/blog/2019/08/27/aws-rds-cross-account-snapshot-restoration/" title="AWS RDS cross account snapshot restoration">AWS RDS cross account snapshot restoration</a></li></ul></li><li><a href="https://opstree.com/blog/author/ashsingh16/"> <img alt='1' src='https://secure.gravatar.com/avatar/fbcc7e692990c92cfcf322ddfb7b265c?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/fbcc7e692990c92cfcf322ddfb7b265c?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ashwani Singh</strong></a><ul><li><a href="https://opstree.com/blog/2024/04/16/istio-circuit-breaker-when-failure-is-a-better-option/" title="Istio Circuit Breaker &#8211; When Failure is a Better Option">Istio Circuit Breaker &#8211; When Failure is a Better Option</a></li></ul></li><li><a href="https://opstree.com/blog/author/ashutoshyadav66/"> <img alt='1' src='https://secure.gravatar.com/avatar/9028b66d911935e2fbed4a0294d3ada9?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/9028b66d911935e2fbed4a0294d3ada9?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ashutosh Yadav</strong></a><ul><li><a href="https://opstree.com/blog/2024/02/13/navigating-aws-finops-harnessing-cloud-intelligence-dashboards-for-strategic-cost-optimization/" title="Navigating AWS FinOps: Harnessing Cloud Intelligence Dashboards for Strategic Cost Optimization">Navigating AWS FinOps: Harnessing Cloud Intelligence Dashboards for Strategic Cost Optimization</a></li><li><a href="https://opstree.com/blog/2024/01/02/architecting-success-best-practices-for-implementing-aws-control-tower/" title="Architecting Success: Best Practices for Implementing AWS Control Tower">Architecting Success: Best Practices for Implementing AWS Control Tower</a></li><li><a href="https://opstree.com/blog/2023/12/19/mastering-aws-rds-backups-navigating-encryption-challenges-with-aws-key-management-service-kms/" title="Mastering AWS RDS Backups: Navigating Encryption Challenges with AWS Key Management Service (KMS)">Mastering AWS RDS Backups: Navigating Encryption Challenges with AWS Key Management Service (KMS)</a></li><li><a href="https://opstree.com/blog/2023/01/10/group-based-authorization-in-gitlab/" title="Group-Based Authorization in GitLab">Group-Based Authorization in GitLab</a></li></ul></li><li><a href="https://opstree.com/blog/author/avoril/"> <img alt='1' src='https://secure.gravatar.com/avatar/766a44bc759223d22309683bc9a31feb?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/766a44bc759223d22309683bc9a31feb?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Avinaw Sharma</strong></a><ul><li><a href="https://opstree.com/blog/2021/09/28/traefik-a-reverse-proxy-load-balancer/" title="Traefik a Reverse Proxy/Load Balancer">Traefik a Reverse Proxy/Load Balancer</a></li></ul></li><li><a href="https://opstree.com/blog/author/ayushyadav/"> <img alt='1' src='https://secure.gravatar.com/avatar/33cfa9a9d33737b64fe94c4f83ce2b42?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/33cfa9a9d33737b64fe94c4f83ce2b42?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ayush Yadav</strong></a><ul><li><a href="https://opstree.com/blog/2024/07/16/lambda-function-setup-guide-for-security-group-event-notifications-in-slack/" title="Lambda Function Setup Guide for Security Group Event Notifications in Slack">Lambda Function Setup Guide for Security Group Event Notifications in Slack</a></li><li><a href="https://opstree.com/blog/2024/07/09/lambda-function-setup-guide-for-iam-event-notifications-in-slack/" title="Lambda Function Setup Guide for IAM Event Notifications in Slack">Lambda Function Setup Guide for IAM Event Notifications in Slack</a></li><li><a href="https://opstree.com/blog/2024/04/17/migrate-azure-to-aws-using-application-migration-service/" title="Migrate Azure to AWS using Application Migration Service">Migrate Azure to AWS using Application Migration Service</a></li></ul></li><li><a href="https://opstree.com/blog/author/azharalisnaatak/"> <img alt='1' src='https://secure.gravatar.com/avatar/30c2cccbf4940ff24b1455f7a59cd22d?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/30c2cccbf4940ff24b1455f7a59cd22d?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Azhar Ali</strong></a><ul><li><a href="https://opstree.com/blog/2024/09/10/sharing-aws-encrypted-rds-snapshot-between-two-accounts/" title="Sharing AWS Encrypted RDS Snapshot Between Two Accounts.">Sharing AWS Encrypted RDS Snapshot Between Two Accounts.</a></li><li><a href="https://opstree.com/blog/2024/09/03/blocking-web-traffic-with-waf-in-aws/" title="Blocking Web Traffic With WAF In AWS">Blocking Web Traffic With WAF In AWS</a></li><li><a href="https://opstree.com/blog/2024/08/27/setup-cross-region-replication-in-s3/" title="Setup Cross Region Replication in S3">Setup Cross Region Replication in S3</a></li><li><a href="https://opstree.com/blog/2024/04/04/migration-of-ms-sql-from-azure-vm-to-amazon-rds/" title="Migration Of MS SQL From Azure VM TO Amazon RDS">Migration Of MS SQL From Azure VM TO Amazon RDS</a></li></ul></li><li><a href="https://opstree.com/blog/author/balpreetbanga/"> <img alt='1' src='https://secure.gravatar.com/avatar/c184b141f2e40340df029d6c35f656a6?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/c184b141f2e40340df029d6c35f656a6?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>balpreetbanga</strong></a><ul><li><a href="https://opstree.com/blog/2022/10/04/prometheus-and-grafana-on-kubernetes/" title="Deploying Prometheus and Grafana on Kubernetes">Deploying Prometheus and Grafana on Kubernetes</a></li><li><a href="https://opstree.com/blog/2022/03/29/statefulsets-in-k8s/" title="Explore More on StatefulSets in K8s">Explore More on StatefulSets in K8s</a></li></ul></li><li><a href="https://opstree.com/blog/author/bhanukumari27a3779439/"> <img alt='1' src='https://secure.gravatar.com/avatar/2c1422d5980debe6087b30be86788e25?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/2c1422d5980debe6087b30be86788e25?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Bhanu Kumari</strong></a><ul><li><a href="https://opstree.com/blog/2024/02/27/ci-cd-with-github-actions-concepts/" title="CI/CD with GitHub Actions &#8211; Concepts">CI/CD with GitHub Actions &#8211; Concepts</a></li></ul></li><li><a href="https://opstree.com/blog/author/bhupendersinghb5dca0b393/"> <img alt='1' src='https://secure.gravatar.com/avatar/356475eba7b59791e71a311770a375e0?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/356475eba7b59791e71a311770a375e0?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Bhupender rawat</strong></a><ul><li><a href="https://opstree.com/blog/2023/03/21/cert-manager-issuer-for-cross-account-route-53-eks/" title="Cert-Manager Issuer for Cross-Account Route 53 [ EKS ]">Cert-Manager Issuer for Cross-Account Route 53 [ EKS ]</a></li><li><a href="https://opstree.com/blog/2022/10/18/understanding-ansible-helm-diff-plugin/" title="Understanding Ansible: Helm diff plugin">Understanding Ansible: Helm diff plugin</a></li><li><a href="https://opstree.com/blog/2022/08/09/a-step-by-step-guide-to-integrate-azure-active-directory-with-redash-saml-sso/" title="A Step-by-Step Guide to Integrate Azure Active Directory with Redash SAML [ SSO ]">A Step-by-Step Guide to Integrate Azure Active Directory with Redash SAML [ SSO ]</a></li><li><a href="https://opstree.com/blog/2022/04/26/google-python-api-the-easy-way/" title="Google Python API: The easy way">Google Python API: The easy way</a></li><li><a href="https://opstree.com/blog/2021/12/07/kubernetes-daemonset/" title="Kubernetes: DaemonSet">Kubernetes: DaemonSet</a></li></ul></li><li><a href="https://opstree.com/blog/author/bishtgeeta/"> <img alt='1' src='https://secure.gravatar.com/avatar/199104af8cf90735e366aa57c3b3ab60?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/199104af8cf90735e366aa57c3b3ab60?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>bishtgeeta</strong></a><ul><li><a href="https://opstree.com/blog/2022/01/11/four-main-metrics-of-prometheus/" title="Four Main Metrics of Prometheus">Four Main Metrics of Prometheus</a></li></ul></li><li><a href="https://opstree.com/blog/author/chauhananant/"> <img alt='1' src='https://secure.gravatar.com/avatar/63ced52303308706a109ace586153220?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/63ced52303308706a109ace586153220?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Anant</strong></a><ul><li><a href="https://opstree.com/blog/2023/11/07/enabling-cors-on-azure-apim/" title="Enabling CORS on Azure APIM">Enabling CORS on Azure APIM</a></li><li><a href="https://opstree.com/blog/2023/05/16/azure-cdn/" title="Azure CDN">Azure CDN</a></li></ul></li><li><a href="https://opstree.com/blog/author/choprasahab/"> <img alt='1' src='https://secure.gravatar.com/avatar/8a9a6eb3734ffd292280296bfd562979?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/8a9a6eb3734ffd292280296bfd562979?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Kartik Chopra</strong></a><ul><li><a href="https://opstree.com/blog/2020/02/03/what-why-and-how-of-ctf-challenges/" title="What, Why and How of CTF Challenges?">What, Why and How of CTF Challenges?</a></li><li><a href="https://opstree.com/blog/2020/01/07/recap-amrita-inctf-2019-part-1/" title="Recap Amrita InCTF 2019 | Part 1">Recap Amrita InCTF 2019 | Part 1</a></li></ul></li><li><a href="https://opstree.com/blog/author/deepakgupta97/"> <img alt='1' src='https://secure.gravatar.com/avatar/fdadc27f343143301e06cab928173ad0?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/fdadc27f343143301e06cab928173ad0?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Deepak Gupta</strong></a><ul><li><a href="https://opstree.com/blog/2022/03/08/introducing-opstree-tomcat-image/" title="Introducing OpsTree Tomcat Image">Introducing OpsTree Tomcat Image</a></li><li><a href="https://opstree.com/blog/2021/08/31/provisioning-infra-and-deployments-in-aws-using-packer-terraform-and-jenkins/" title="Provisioning Infra and Deployments In AWS : Using Packer, Terraform and Jenkins">Provisioning Infra and Deployments In AWS : Using Packer, Terraform and Jenkins</a></li><li><a href="https://opstree.com/blog/2021/07/27/introduction-to-prometheus-monitoring/" title="Introduction to Prometheus Monitoring">Introduction to Prometheus Monitoring</a></li><li><a href="https://opstree.com/blog/2021/04/20/docker-buildkit-faster-builds-mounts-and-features/" title="Docker BuildKit : Faster Builds, Mounts and Features">Docker BuildKit : Faster Builds, Mounts and Features</a></li><li><a href="https://opstree.com/blog/2021/03/23/create-your-first-helm-chart-part-03/" title="Create Your First Helm Chart (Part 03)">Create Your First Helm Chart (Part 03)</a></li></ul></li><li><a href="https://opstree.com/blog/author/deepaknishad/"> <img alt='1' src='https://secure.gravatar.com/avatar/f237ebcccdd8f32f82383e357e9e2f55?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/f237ebcccdd8f32f82383e357e9e2f55?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Deepak Nishad</strong></a><ul><li><a href="https://opstree.com/blog/2024/11/12/understanding-cow-and-mor-in-apache-hudi-choosing-the-right-storage-strategy/" title="Understanding COW and MOR in Apache Hudi: Choosing the Right Storage Strategy ">Understanding COW and MOR in Apache Hudi: Choosing the Right Storage Strategy </a></li><li><a href="https://opstree.com/blog/2024/08/06/uploading-files-using-pre-signed-urls-to-a-specific-storage-class/" title="Uploading Files Using Pre-Signed URLs to a Specific Storage Class">Uploading Files Using Pre-Signed URLs to a Specific Storage Class</a></li><li><a href="https://opstree.com/blog/2024/07/30/use-case-backup-and-replication-setup-between-ec2-mysql-and-rds-mysql/" title="Use Case: Backup and Replication Setup Between EC2 MySQL and RDS MySQL">Use Case: Backup and Replication Setup Between EC2 MySQL and RDS MySQL</a></li><li><a href="https://opstree.com/blog/2024/07/23/comparison-between-mydumper-mysqldump-xtrabackup/" title="Comparison between Mydumper, mysqldump, xtrabackup">Comparison between Mydumper, mysqldump, xtrabackup</a></li></ul></li><li><a href="https://opstree.com/blog/author/dev-kumar-gautam/"> <img alt='1' src='https://secure.gravatar.com/avatar/e7b872b15b8dff69cf6318bb6879507e?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/e7b872b15b8dff69cf6318bb6879507e?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Dev Kumar Gautam</strong></a><ul><li><a href="https://opstree.com/blog/2024/11/26/understanding-oai-and-oac-in-aws-cloudfront-concepts-configuration-and-best-practices/" title="Understanding OAI and OAC in AWS CloudFront: Concepts, Configuration, and Best Practices">Understanding OAI and OAC in AWS CloudFront: Concepts, Configuration, and Best Practices</a></li></ul></li><li><a href="https://opstree.com/blog/author/devopsninja1/"> <img alt='1' src='https://secure.gravatar.com/avatar/6d376e8fbea2051c7c225a7e1bf6f970?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/6d376e8fbea2051c7c225a7e1bf6f970?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Devesh Sharma</strong></a><ul><li><a href="https://opstree.com/blog/2020/05/12/fasten-docker-build/" title="Fasten Docker build">Fasten Docker build</a></li><li><a href="https://opstree.com/blog/2020/04/14/create-your-own-container-using-linux-namespaces-part-1/" title="Create Your Own Container Using Linux Namespaces Part-1.">Create Your Own Container Using Linux Namespaces Part-1.</a></li><li><a href="https://opstree.com/blog/2020/02/25/raktbeej-proxy/" title="Raktbeej Proxy">Raktbeej Proxy</a></li><li><a href="https://opstree.com/blog/2020/01/29/recap-amrita-inctf-2019-part-2/" title="Recap Amrita InCTF 2019 | Part 2">Recap Amrita InCTF 2019 | Part 2</a></li></ul></li><li><a href="https://opstree.com/blog/author/devpokhariya/"> <img alt='1' src='https://secure.gravatar.com/avatar/3ab752dc7d4d142d06a1ee3e2f6e612e?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/3ab752dc7d4d142d06a1ee3e2f6e612e?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Dev Pokhariya</strong></a><ul><li><a href="https://opstree.com/blog/2021/12/28/kafkas-solution-event-driven-architecture-otkafkadiaries/" title="Kafka’s Solution : Event Driven Architecture: OTKafkaDiaries">Kafka’s Solution : Event Driven Architecture: OTKafkaDiaries</a></li><li><a href="https://opstree.com/blog/2021/02/02/introduction-to-kafka-otkafkadiaries/" title="Introduction To KAFKA: OTKafkaDiaries">Introduction To KAFKA: OTKafkaDiaries</a></li></ul></li><li><a href="https://opstree.com/blog/author/giteshsatywali5debccf5d5/"> <img alt='1' src='https://secure.gravatar.com/avatar/f7421a43dcc20e5e5e038309503c1d4b?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/f7421a43dcc20e5e5e038309503c1d4b?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Gitesh Satywali</strong></a><ul><li><a href="https://opstree.com/blog/2019/05/28/where-there-is-a-shell-there-is-a-way/" title="Where there is a shell, There is a way.">Where there is a shell, There is a way.</a></li></ul></li><li><a href="https://opstree.com/blog/author/guestwriteropstree/"> <img alt='1' src='https://secure.gravatar.com/avatar/b59a47d72b3b1fc82953c41603eaef60?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/b59a47d72b3b1fc82953c41603eaef60?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>guestwriteropstree</strong></a><ul><li><a href="https://opstree.com/blog/2022/06/24/what-is-the-difference-between-cloudops-and-devops/" title="What Is the Difference Between CloudOps And DevOps?">What Is the Difference Between CloudOps And DevOps?</a></li></ul></li><li><a href="https://opstree.com/blog/author/gveeru/"> <img alt='1' src='https://secure.gravatar.com/avatar/bdc3eb4ed619c855a08200424081468d?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/bdc3eb4ed619c855a08200424081468d?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>gveeru</strong></a><ul><li><a href="https://opstree.com/blog/2021/07/20/vpn-services-comparision-how-to-find-the-best-vpn-for-your-business/" title="VPN Services Comparison- How to find the best VPN for your business?">VPN Services Comparison- How to find the best VPN for your business?</a></li></ul></li><li><a href="https://opstree.com/blog/author/harshit-singh/"> <img alt='1' src='https://secure.gravatar.com/avatar/5095710cfb67ada0bfd7a327ff16b2c5?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/5095710cfb67ada0bfd7a327ff16b2c5?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Harshit Singh</strong></a><ul><li><a href="https://opstree.com/blog/2024/12/10/securing-software-supply-chains-with-slsa/" title="Securing Software Supply Chains with SLSA">Securing Software Supply Chains with SLSA</a></li></ul></li><li><a href="https://opstree.com/blog/author/himanshiparnami/"> <img alt='1' src='https://secure.gravatar.com/avatar/24416e151e3753a10588e3705fcea587?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/24416e151e3753a10588e3705fcea587?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>himanshiparnami</strong></a><ul><li><a href="https://opstree.com/blog/2023/02/14/self-hosted-gitlab-migration-part-1/" title="Self-Hosted GitLab Migration &#8211; Part 1">Self-Hosted GitLab Migration &#8211; Part 1</a></li><li><a href="https://opstree.com/blog/2022/12/20/deploying-azure-policy-using-terraform-module/" title="Deploying Azure Policy using Terraform Module">Deploying Azure Policy using Terraform Module</a></li><li><a href="https://opstree.com/blog/2022/08/16/how-to-setup-consul-through-the-osm-ansible-role/" title="How to Setup Consul through the OSM Ansible Role">How to Setup Consul through the OSM Ansible Role</a></li></ul></li><li><a href="https://opstree.com/blog/author/himanshumudgal08/"> <img alt='1' src='https://secure.gravatar.com/avatar/7b8705d543aae6638e1f12c79f06a717?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/7b8705d543aae6638e1f12c79f06a717?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>himanshumudgal08</strong></a><ul><li><a href="https://opstree.com/blog/2022/11/29/deploying-terraform-iac-using-azure-devops-runtime-parameters/" title="Deploying Terraform IAC Using Azure DevOps Runtime Parameters">Deploying Terraform IAC Using Azure DevOps Runtime Parameters</a></li><li><a href="https://opstree.com/blog/2022/09/20/increasing-code-reusability-using-task-groups-in-azure-devops/" title="Increasing Code Reusability Using Task Groups in Azure DevOps">Increasing Code Reusability Using Task Groups in Azure DevOps</a></li><li><a href="https://opstree.com/blog/2022/08/30/how-to-setup-an-agent-on-azure-devops/" title="How To Setup An Agent On Azure Devops">How To Setup An Agent On Azure Devops</a></li><li><a href="https://opstree.com/blog/2022/06/28/terraform-ci-cd-with-azure-devops/" title="Terraform CI-CD With Azure DevOps">Terraform CI-CD With Azure DevOps</a></li><li><a href="https://opstree.com/blog/2022/06/21/what-is-a-bare-git-repository/" title="What is a Bare Git Repository?">What is a Bare Git Repository?</a></li></ul></li><li><a href="https://opstree.com/blog/author/himanshuopstree/"> <img alt='1' src='https://secure.gravatar.com/avatar/5ad028a3b47c0d2c77d5d3a6cbe514fa?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/5ad028a3b47c0d2c77d5d3a6cbe514fa?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Himanshu Uniyal</strong></a><ul><li><a href="https://opstree.com/blog/2021/05/11/taints-and-tolerations-usage-with-node-selector-in-kubernetes-scheduling/" title="Taints and Tolerations Usage with Node Selector in Kubernetes Scheduling">Taints and Tolerations Usage with Node Selector in Kubernetes Scheduling</a></li></ul></li><li><a href="https://opstree.com/blog/author/iamrajatvats/"> <img alt='1' src='https://secure.gravatar.com/avatar/3e82dc4ff64b8b99aff476251dc479a0?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/3e82dc4ff64b8b99aff476251dc479a0?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Rajat Vats</strong></a><ul><li><a href="https://opstree.com/blog/2020/10/27/how-to-implement-ci-cd-using-aws-codebuild-codedeploy-and-codepipeline/" title="How to implement CI/CD using AWS CodeBuild, CodeDeploy and CodePipeline">How to implement CI/CD using AWS CodeBuild, CodeDeploy and CodePipeline</a></li><li><a href="https://opstree.com/blog/2020/09/01/why-we-should-use-transit-direct-connect-gateways/" title="Why We Should Use Transit &amp; Direct Connect Gateways!">Why We Should Use Transit &amp; Direct Connect Gateways!</a></li></ul></li><li><a href="https://opstree.com/blog/author/iamvikasbisht/"> <img alt='1' src='https://secure.gravatar.com/avatar/d188a527a2f683eac8b1e2376277adad?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/d188a527a2f683eac8b1e2376277adad?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>iamvikasbisht</strong></a><ul><li><a href="https://opstree.com/blog/2023/04/18/github-self-hosted-runner-on-kubernetes/" title="GitHub: Self-Hosted Runner on Kubernetes">GitHub: Self-Hosted Runner on Kubernetes</a></li><li><a href="https://opstree.com/blog/2022/04/12/know-the-role-of-k8s-service-account-in-granting-access/" title="Know the Role of K8S Service Account in Granting Access">Know the Role of K8S Service Account in Granting Access</a></li><li><a href="https://opstree.com/blog/2021/11/23/debugging-in-shell-script/" title="Debugging in Shell Script">Debugging in Shell Script</a></li></ul></li><li><a href="https://opstree.com/blog/author/ish149/"> <img alt='1' src='https://secure.gravatar.com/avatar/ce9105efbdf9dfba248d42f8bd747b7e?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/ce9105efbdf9dfba248d42f8bd747b7e?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Isha sharma</strong></a><ul><li><a href="https://opstree.com/blog/2021/12/21/fresh-service-the-best-tool-to-manage-it-operations/" title="Fresh Service &#8211; MY Experience with Analytics &amp; Workflow Automator Features">Fresh Service &#8211; MY Experience with Analytics &amp; Workflow Automator Features</a></li></ul></li><li><a href="https://opstree.com/blog/author/ishaanambashta/"> <img alt='1' src='https://secure.gravatar.com/avatar/33d43470ba1aeff4e119252c598d23d8?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/33d43470ba1aeff4e119252c598d23d8?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ishaan Ambashta</strong></a><ul><li><a href="https://opstree.com/blog/2022/09/13/monitoring-and-release-tracking-with-sentry/" title="Monitoring and Release tracking with Sentry">Monitoring and Release tracking with Sentry</a></li></ul></li><li><a href="https://opstree.com/blog/author/ishanjiconnect/"> <img alt='1' src='https://secure.gravatar.com/avatar/48b925bdba49ce4f97f672befa9b8cb9?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/48b925bdba49ce4f97f672befa9b8cb9?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>ishanjiconnect</strong></a><ul><li><a href="https://opstree.com/blog/2019/11/19/error-handling-in-ansible/" title="ERROR HANDLING IN ANSIBLE">ERROR HANDLING IN ANSIBLE</a></li><li><a href="https://opstree.com/blog/2019/07/02/speeding-up-ansible-execution-part-2/" title="Speeding up Ansible Execution Part 2">Speeding up Ansible Execution Part 2</a></li><li><a href="https://opstree.com/blog/2019/06/25/speeding-up-ansible-execution-part-1/" title="Speeding up Ansible Execution Part 1">Speeding up Ansible Execution Part 1</a></li></ul></li><li><a href="https://opstree.com/blog/author/jaiswalshikha/"> <img alt='1' src='https://secure.gravatar.com/avatar/27ae662365bff6aabba46f5c865f08f0?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/27ae662365bff6aabba46f5c865f08f0?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Shikha Jaiswal</strong></a><ul><li><a href="https://opstree.com/blog/2021/08/24/proc-file-system-in-linux/" title="Proc File System in Linux">Proc File System in Linux</a></li></ul></li><li><a href="https://opstree.com/blog/author/jassingh29/"> <img alt='1' src='https://secure.gravatar.com/avatar/8a55e7767bddd65be95d232903fe2263?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/8a55e7767bddd65be95d232903fe2263?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Jaspreet Singh</strong></a><ul><li><a href="https://opstree.com/blog/2020/06/23/automatically-backup-restore-alibaba-mysql-using-grandfather-father-son-strategy/" title="Automatically Backup Alibaba MySQL using Grandfather-Father-Son Strategy">Automatically Backup Alibaba MySQL using Grandfather-Father-Son Strategy</a></li><li><a href="https://opstree.com/blog/2019/12/17/collect-logs-with-fluentd-in-k8s-part-2/" title="Collect Logs with Fluentd in K8s. (Part-2)">Collect Logs with Fluentd in K8s. (Part-2)</a></li><li><a href="https://opstree.com/blog/2019/12/10/__trashed/" title="EFK 7.4.0 Stack on Kubernetes. (Part-1)">EFK 7.4.0 Stack on Kubernetes. (Part-1)</a></li></ul></li><li><a href="https://opstree.com/blog/author/javedkhan0749/"> <img alt='1' src='https://secure.gravatar.com/avatar/b8b39cd0befb9ceedde9410d70bfb6aa?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/b8b39cd0befb9ceedde9410d70bfb6aa?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>javedkhan0749</strong></a><ul><li><a href="https://opstree.com/blog/2023/06/13/aws-site-to-site-vpn-connection/" title="AWS Site-to-Site VPN Connection">AWS Site-to-Site VPN Connection</a></li></ul></li><li><a href="https://opstree.com/blog/author/kapendrasingh/"> <img alt='1' src='https://secure.gravatar.com/avatar/52528ee3247b1e8a6a41b09f380878dc?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/52528ee3247b1e8a6a41b09f380878dc?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Kapendra Singh</strong></a><ul><li><a href="https://opstree.com/blog/2020/03/18/terraform-workspace-multiple-environment/" title="Terraform WorkSpace &#8211; Multiple Environment">Terraform WorkSpace &#8211; Multiple Environment</a></li><li><a href="https://opstree.com/blog/2019/09/24/mysql-data-at-rest-encryption/" title="The Concept Of Data At Rest Encryption In MySql">The Concept Of Data At Rest Encryption In MySql</a></li></ul></li><li><a href="https://opstree.com/blog/author/kartikeygupta/"> <img alt='1' src='https://secure.gravatar.com/avatar/d68c2d4484b3c7b62ab4f9cb255b4a8e?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/d68c2d4484b3c7b62ab4f9cb255b4a8e?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Kartikey Gupta</strong></a><ul><li><a href="https://opstree.com/blog/2019/06/11/what-without-internet/" title="What Without Internet">What Without Internet</a></li></ul></li><li><a href="https://opstree.com/blog/author/kirtinehra/"> <img alt='1' src='https://secure.gravatar.com/avatar/e61bf6d3f0cf884284ebdaf164be3c92?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/e61bf6d3f0cf884284ebdaf164be3c92?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>kirtinehra</strong></a><ul><li><a href="https://opstree.com/blog/2022/12/13/trigger-jenkins-job-using-aws-lambda-triggered-by-s3-event/" title="Trigger Jenkins Job using AWS Lambda triggered by S3 Event">Trigger Jenkins Job using AWS Lambda triggered by S3 Event</a></li><li><a href="https://opstree.com/blog/2021/11/09/nginx-monitoring-using-telegraf-prometheus-grafana/" title="Nginx monitoring using Telegraf/Prometheus/Grafana">Nginx monitoring using Telegraf/Prometheus/Grafana</a></li><li><a href="https://opstree.com/blog/2021/07/06/autoscale-azure-mysql-server-using-azure-automation-account-and-powershell/" title="Autoscaling Azure MySql Server using Azure Automation">Autoscaling Azure MySql Server using Azure Automation</a></li></ul></li><li><a href="https://opstree.com/blog/author/komaljaiswal/"> <img alt='1' src='https://secure.gravatar.com/avatar/69e807be0373a03ddc373c87bf467b0e?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/69e807be0373a03ddc373c87bf467b0e?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Komal Jaiswal</strong></a><ul><li><a href="https://opstree.com/blog/2024/12/03/ctrlshiftepic-deployment-strategies-unleashed/" title="Ctrl+Shift+Epic : Deployment Strategies Unleashed">Ctrl+Shift+Epic : Deployment Strategies Unleashed</a></li><li><a href="https://opstree.com/blog/2024/10/08/prometheus-the-prom-king-part-2/" title="Prometheus — The Prom King (Part 2)">Prometheus — The Prom King (Part 2)</a></li><li><a href="https://opstree.com/blog/2024/10/01/a-fun-and-easy-guide-to-monitoring-and-observability-with-prometheus/" title="A Fun and Easy Guide to Monitoring and Observability With Prometheus">A Fun and Easy Guide to Monitoring and Observability With Prometheus</a></li><li><a href="https://opstree.com/blog/2024/09/17/what-are-kubernetes-events/" title="What are Kubernetes Events ?">What are Kubernetes Events ?</a></li><li><a href="https://opstree.com/blog/2024/08/20/helm-in-kubernetes/" title="What is Helm in Kubernetes ?">What is Helm in Kubernetes ?</a></li></ul></li><li><a href="https://opstree.com/blog/author/komalprabhakaropstree/"> <img alt='1' src='https://secure.gravatar.com/avatar/840d9f0059bcfdc4f05d8776354c8bbe?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/840d9f0059bcfdc4f05d8776354c8bbe?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Komal J. Prabhakar</strong></a><ul><li><a href="https://opstree.com/blog/2024/01/30/navigating-compliance-landscape-in-fintech/" title="Navigating Compliance Landscape in Fintech">Navigating Compliance Landscape in Fintech</a></li><li><a href="https://opstree.com/blog/2023/10/31/unpacking-our-findings-from-assessing-numerous-infrastructures-part-2/" title="Unpacking Our Findings From Assessing Numerous Infrastructures &#8211; Part 2">Unpacking Our Findings From Assessing Numerous Infrastructures &#8211; Part 2</a></li><li><a href="https://opstree.com/blog/2023/10/03/unpacking-our-findings-from-assessing-numerous-infrastructures-part-1/" title="Unpacking Our Findings From Assessing Numerous Infrastructures &#8211; Part 1">Unpacking Our Findings From Assessing Numerous Infrastructures &#8211; Part 1</a></li><li><a href="https://opstree.com/blog/2023/04/28/technical-roadblocks-in-edtech-strategies-for-success/" title="Technical Roadblocks in Edtech: Strategies for Success">Technical Roadblocks in Edtech: Strategies for Success</a></li></ul></li><li><a href="https://opstree.com/blog/author/kritarth29/"> <img alt='1' src='https://secure.gravatar.com/avatar/11ecc2a8728c57219bba283e2692357f?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/11ecc2a8728c57219bba283e2692357f?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>kritarth29</strong></a><ul><li><a href="https://opstree.com/blog/2023/02/28/introduction-to-azure-active-directory/" title="Introduction to Azure Active Directory">Introduction to Azure Active Directory</a></li><li><a href="https://opstree.com/blog/2022/09/06/introduction-to-azure-security/" title="Introduction to Azure Security">Introduction to Azure Security</a></li><li><a href="https://opstree.com/blog/2022/02/15/bigbulls-game-series-patching-mongodb-using-ansible/" title="BigBulls Game Series- Patching MongoDB using Ansible">BigBulls Game Series- Patching MongoDB using Ansible</a></li><li><a href="https://opstree.com/blog/2021/11/30/ec2-store-overview-difference-b-w-aws-ebs-and-instance-store/" title="EC2 STORE OVERVIEW- Difference B/W AWS EBS And Instance Store">EC2 STORE OVERVIEW- Difference B/W AWS EBS And Instance Store</a></li><li><a href="https://opstree.com/blog/2021/11/02/your-guide-for-patching-elastic-search/" title="Your Guide for Patching Elastic Search!">Your Guide for Patching Elastic Search!</a></li></ul></li><li><a href="https://opstree.com/blog/author/kuldeep6392/"> <img alt='1' src='https://secure.gravatar.com/avatar/002f52e52fc5093f167ecf3fcfa6dde9?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/002f52e52fc5093f167ecf3fcfa6dde9?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>kuldeep Singh</strong></a><ul><li><a href="https://opstree.com/blog/2023/08/29/azure-conditional-access-fortifying-your-defense-strategy-for-modern-security-challenges/" title="Azure Conditional Access: Fortifying Your Defense Strategy for Modern Security Challenges">Azure Conditional Access: Fortifying Your Defense Strategy for Modern Security Challenges</a></li><li><a href="https://opstree.com/blog/2023/05/30/basic-logging-setup-of-loki-grafana/" title="Basic Logging Setup of Loki Grafana">Basic Logging Setup of Loki Grafana</a></li></ul></li><li><a href="https://opstree.com/blog/author/lakshayarora1/"> <img alt='1' src='https://secure.gravatar.com/avatar/5d366fea7c10c5d85a31bdd7b8d0d2f7?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/5d366fea7c10c5d85a31bdd7b8d0d2f7?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>lakshayarora1</strong></a><ul><li><a href="https://opstree.com/blog/2021/06/29/using-trufflehog-utility-in-your-jenkins-pipeline/" title="Using TruffleHog Utility in Your Jenkins Pipeline">Using TruffleHog Utility in Your Jenkins Pipeline</a></li><li><a href="https://opstree.com/blog/2021/04/27/sonarqube-integration-with-azure-devops/" title="SonarQube Integration with Azure DevOps">SonarQube Integration with Azure DevOps</a></li><li><a href="https://opstree.com/blog/2020/12/22/an-overview-of-logic-apps-with-its-use-cases/" title="An Overview of Logic Apps with its Use Cases">An Overview of Logic Apps with its Use Cases</a></li></ul></li><li><a href="https://opstree.com/blog/author/likithrk198c0f0369/"> <img alt='1' src='https://secure.gravatar.com/avatar/0bd97dadbc77e080da7ddf9b1be3beaa?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/0bd97dadbc77e080da7ddf9b1be3beaa?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Likith R K</strong></a><ul><li><a href="https://opstree.com/blog/2022/03/22/introduction-to-buildkite/" title="Know How to Get Started with Buildkite">Know How to Get Started with Buildkite</a></li></ul></li><li><a href="https://opstree.com/blog/author/lovedeepsharma789/"> <img alt='1' src='https://secure.gravatar.com/avatar/52bbd6f88135c3618cc8606082332d57?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/52bbd6f88135c3618cc8606082332d57?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Lovedeep Sharma</strong></a><ul><li><a href="https://opstree.com/blog/2019/02/21/best-practices-of-ansible-role/" title="Best Practices of Ansible Role">Best Practices of Ansible Role</a></li><li><a href="https://opstree.com/blog/2018/10/22/docker-logging-driver/" title="Docker Logging Driver">Docker Logging Driver</a></li></ul></li><li><a href="https://opstree.com/blog/author/maheshkumar38/"> <img alt='1' src='https://secure.gravatar.com/avatar/f9d862e412ed12b6f0e4949a0a1ec94f?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/f9d862e412ed12b6f0e4949a0a1ec94f?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Mahesh Kumar</strong></a><ul><li><a href="https://opstree.com/blog/2022/11/08/what-is-sre-site-reliability-engineer/" title="What is SRE (Site Reliability Engineer)">What is SRE (Site Reliability Engineer)</a></li><li><a href="https://opstree.com/blog/2022/08/23/a-detailed-guide-to-key-metrics-of-mongodb-monitoring/" title="A Detailed Guide to Key Metrics of MongoDB Monitoring">A Detailed Guide to Key Metrics of MongoDB Monitoring</a></li><li><a href="https://opstree.com/blog/2019/12/03/prometheus-alertmanager-integration-with-ms-teams/" title="Prometheus-Alertmanager integration with MS-teams">Prometheus-Alertmanager integration with MS-teams</a></li></ul></li><li><a href="https://opstree.com/blog/author/mishraankit007/"> <img alt='1' src='https://secure.gravatar.com/avatar/cba69c3b014ed34fc5d09548568e6cae?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/cba69c3b014ed34fc5d09548568e6cae?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ankit Kumar Mishra</strong></a><ul><li><a href="https://opstree.com/blog/2023/09/05/gcp-to-azure-vpn-tunneling-with-multiple-networks/" title="GCP to Azure VPN Tunneling with Multiple Networks">GCP to Azure VPN Tunneling with Multiple Networks</a></li></ul></li><li><a href="https://opstree.com/blog/author/mohitkanojia/"> <img alt='1' src='https://secure.gravatar.com/avatar/21fc0b696d91f1a67ba3633865874cd4?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/21fc0b696d91f1a67ba3633865874cd4?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>mohitkanojia</strong></a><ul><li><a href="https://opstree.com/blog/2021/06/08/servicenow-integration-with-azure-alerts-step-by-step-setup/" title="ServiceNow Integration with Azure Alerts &#8211; Step By Step Setup">ServiceNow Integration with Azure Alerts &#8211; Step By Step Setup</a></li></ul></li><li><a href="https://opstree.com/blog/author/moksh4200/"> <img alt='1' src='https://secure.gravatar.com/avatar/c2f318d919d6ff940b8c16a43ce2cfc3?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/c2f318d919d6ff940b8c16a43ce2cfc3?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Moksh Bhardwaj</strong></a><ul><li><a href="https://opstree.com/blog/2024/02/20/nifi-and-zookeeper-cluster-setup-with-terraform/" title="Nifi and Zookeeper Cluster Setup with Terraform">Nifi and Zookeeper Cluster Setup with Terraform</a></li><li><a href="https://opstree.com/blog/2023/11/21/nifi-cluster-setup-with-external-zookeeper/" title="Nifi Cluster Setup with External Zookeeper">Nifi Cluster Setup with External Zookeeper</a></li><li><a href="https://opstree.com/blog/2023/11/14/securing-nifi-cluster-with-tls-toolkit/" title="Securing Nifi Cluster with TLS Toolkit">Securing Nifi Cluster with TLS Toolkit</a></li><li><a href="https://opstree.com/blog/2022/03/01/aws-elastic-network-interface/" title="AWS Elastic Network Interface">AWS Elastic Network Interface</a></li></ul></li><li><a href="https://opstree.com/blog/author/moulendu/"> <img alt='1' src='https://secure.gravatar.com/avatar/2116a20a4b4cd4df0f188238b62e657c?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/2116a20a4b4cd4df0f188238b62e657c?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>moulendu</strong></a><ul><li><a href="https://opstree.com/blog/2024/06/25/introduction-to-inodes/" title="Introduction To Inodes">Introduction To Inodes</a></li></ul></li><li><a href="https://opstree.com/blog/author/mrituyanjaysinha/"> <img alt='1' src='https://secure.gravatar.com/avatar/d1831ddad74cccf6afb47f41927f5a5e?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/d1831ddad74cccf6afb47f41927f5a5e?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>mrituyanjaysinha</strong></a><ul><li><a href="https://opstree.com/blog/2023/10/10/exploring-the-power-of-iam-roles-anywhere/" title="Exploring the Power of IAM Roles Anywhere">Exploring the Power of IAM Roles Anywhere</a></li></ul></li><li><a href="https://opstree.com/blog/author/mukeshtuteja/"> <img alt='1' src='https://secure.gravatar.com/avatar/1f719975a3ed2c329af57a18ed4d2dfd?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/1f719975a3ed2c329af57a18ed4d2dfd?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>mukeshtuteja</strong></a><ul><li><a href="https://opstree.com/blog/2020/03/24/ansible-directory-structure-default-vs-vars/" title="Ansible directory structure (Default vs Vars)">Ansible directory structure (Default vs Vars)</a></li><li><a href="https://opstree.com/blog/2019/04/02/resolving-segmentation-fault-core-dumped-in-ubuntu/" title="Resolving Segmentation Fault (“Core dumped”) in Ubuntu">Resolving Segmentation Fault (“Core dumped”) in Ubuntu</a></li></ul></li><li><a href="https://opstree.com/blog/author/mukuljoshi3c7fb47658/"> <img alt='1' src='https://secure.gravatar.com/avatar/92ece5fc64fc62eff0dbd132ab870249?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/92ece5fc64fc62eff0dbd132ab870249?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Mukul Joshi</strong></a><ul><li><a href="https://opstree.com/blog/2024/01/16/demystifying-ocis-virtual-cloud-network-a-deep-dive-into-vcn-architecture/" title="Demystifying OCI&#8217;s Virtual Cloud Network: A Deep Dive into VCN Architecture (Part 1)">Demystifying OCI&#8217;s Virtual Cloud Network: A Deep Dive into VCN Architecture (Part 1)</a></li></ul></li><li><a href="https://opstree.com/blog/author/naveenverma023/"> <img alt='1' src='https://secure.gravatar.com/avatar/779408028e4d056a969b9a1f31225d1f?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/779408028e4d056a969b9a1f31225d1f?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>naveenverma023</strong></a><ul><li><a href="https://opstree.com/blog/2021/04/13/jenkins-vs-azure-devops/" title="Jenkins vs Azure DevOps">Jenkins vs Azure DevOps</a></li><li><a href="https://opstree.com/blog/2020/11/03/ease-your-azure-infrastructure-with-azure-blueprints/" title="Ease your Azure Infrastructure with Azure Blueprints">Ease your Azure Infrastructure with Azure Blueprints</a></li><li><a href="https://opstree.com/blog/2020/07/14/master-pipelines-with-azure-pipeline-templates/" title="Master Pipelines with Azure Pipeline Templates">Master Pipelines with Azure Pipeline Templates</a></li></ul></li><li><a href="https://opstree.com/blog/author/navneetsingh53b65343c0/"> <img alt='1' src='https://secure.gravatar.com/avatar/229b40997c93c7e27792fd3a1750edf5?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/229b40997c93c7e27792fd3a1750edf5?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Navneet Singh</strong></a><ul><li><a href="https://opstree.com/blog/2022/06/07/amazon-cloudfront/" title="Learn everything about Amazon Cloudfront">Learn everything about Amazon Cloudfront</a></li></ul></li><li><a href="https://opstree.com/blog/author/nderai/"> <img alt='1' src='https://secure.gravatar.com/avatar/2b22bec031e6b16e76dd67fdc6e6539b?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/2b22bec031e6b16e76dd67fdc6e6539b?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>nikki derai</strong></a><ul><li><a href="https://opstree.com/blog/2022/10/11/wazuh-the-siem-platform/" title="Wazuh : The SIEM Platform">Wazuh : The SIEM Platform</a></li></ul></li><li><a href="https://opstree.com/blog/author/nehasinha20/"> <img alt='1' src='https://secure.gravatar.com/avatar/0334176f8963b20189f67c0358dc9727?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/0334176f8963b20189f67c0358dc9727?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Neha Sinha</strong></a><ul><li><a href="https://opstree.com/blog/2024/05/28/solving-timeout-issues-in-python-django-on-kubernetes/" title="Solving Timeout Issues in Python Django on Kubernetes">Solving Timeout Issues in Python Django on Kubernetes</a></li><li><a href="https://opstree.com/blog/2024/01/23/implementation-of-eso-external-secret-operator-with-google-secret-manager/" title="Implementation of ESO (External Secret Operator) with Google Secret Manager ">Implementation of ESO (External Secret Operator) with Google Secret Manager </a></li><li><a href="https://opstree.com/blog/2023/08/08/introduction-to-external-secret-operator/" title="Introduction to External Secret Operator">Introduction to External Secret Operator</a></li></ul></li><li><a href="https://opstree.com/blog/author/omkdesai/"> <img alt='1' src='https://secure.gravatar.com/avatar/18b224464e7f2fd41570db4065561921?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/18b224464e7f2fd41570db4065561921?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>omkdesai</strong></a><ul><li><a href="https://opstree.com/blog/2023/08/15/how-to-get-java-heap-dump-from-kubernetes-container-into-a-local-machine/" title="How to get Java heap dump from Kubernetes container into a local machine?">How to get Java heap dump from Kubernetes container into a local machine?</a></li></ul></li><li><a href="https://opstree.com/blog/author/opstreeblog/"> <img alt='1' src='https://secure.gravatar.com/avatar/9ca482049807a0a99d56c395b9fd1731?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/9ca482049807a0a99d56c395b9fd1731?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>opstreeblog</strong></a><ul><li><a href="https://opstree.com/blog/2019/03/31/the-closer-you-think-you-are-the-less-youll-actually-see/" title="The closer you think you are, the less you&#8217;ll actually see">The closer you think you are, the less you&#8217;ll actually see</a></li><li><a href="https://opstree.com/blog/2019/03/12/migrate-your-data-between-various-databases/" title="Migrate your data between various Databases">Migrate your data between various Databases</a></li><li><a href="https://opstree.com/blog/2019/02/19/git-inside-out/" title="Git Inside Out">Git Inside Out</a></li><li><a href="https://opstree.com/blog/2019/01/21/log-parsing-of-windows-servers-on-instance-termination/" title="Log Parsing of Windows Servers on Instance Termination">Log Parsing of Windows Servers on Instance Termination</a></li><li><a href="https://opstree.com/blog/2018/05/08/prometheus-overview-and-setup/" title="Prometheus Overview and Setup">Prometheus Overview and Setup</a></li></ul></li><li><a href="https://opstree.com/blog/author/otimran/"> <img alt='1' src='https://secure.gravatar.com/avatar/38ae238198787288a3d5d882a155345b?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/38ae238198787288a3d5d882a155345b?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Imran Ahmad</strong></a><ul><li><a href="https://opstree.com/blog/2023/06/27/cicd-for-mobile-app-development-using-capacitor-js-on-azure-devops/" title="CICD for Mobile App Development Using Capacitor JS on Azure DevOps">CICD for Mobile App Development Using Capacitor JS on Azure DevOps</a></li></ul></li><li><a href="https://opstree.com/blog/author/pankajkumar1301/"> <img alt='1' src='https://secure.gravatar.com/avatar/243e2e3c66b39675e3b055ac5bffbf0e?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/243e2e3c66b39675e3b055ac5bffbf0e?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Pankaj Kumar</strong></a><ul><li><a href="https://opstree.com/blog/2020/09/22/stop-wasting-money-start-cost-optimization-for-aws/" title="Stop Wasting Money, Start Cost Optimization for AWS!">Stop Wasting Money, Start Cost Optimization for AWS!</a></li></ul></li><li><a href="https://opstree.com/blog/author/piiiyuushh/"> <img alt='1' src='https://secure.gravatar.com/avatar/3e7e4858de510f72484081d58b3a3f2d?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/3e7e4858de510f72484081d58b3a3f2d?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Piyush Upadhyay</strong></a><ul><li><a href="https://opstree.com/blog/2024/03/05/navigating-cloud-costs-the-power-of-aws-budget-service/" title="Navigating Cloud Costs: The Power of AWS Budget Service">Navigating Cloud Costs: The Power of AWS Budget Service</a></li><li><a href="https://opstree.com/blog/2023/10/24/my-learning-in-migration-of-mysql-from-5-7-to-8-0/" title="My learning in Migration of MySQL from 5.7 to 8.0">My learning in Migration of MySQL from 5.7 to 8.0</a></li><li><a href="https://opstree.com/blog/2023/06/20/database-migration-service-in-aws/" title="Database Migration Service in AWS">Database Migration Service in AWS</a></li><li><a href="https://opstree.com/blog/2023/05/09/how-to-setup-sso-in-jenkins/" title="How to Setup SSO in Jenkins?">How to Setup SSO in Jenkins?</a></li></ul></li><li><a href="https://opstree.com/blog/author/piyushshailly/"> <img alt='1' src='https://secure.gravatar.com/avatar/68b794ac2d835b575ea988aff5f9de5f?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/68b794ac2d835b575ea988aff5f9de5f?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>piyushshailly</strong></a><ul><li><a href="https://opstree.com/blog/2024/06/11/aws-firewall-samurai-warriors/" title="AWS Firewall- Samurai Warriors">AWS Firewall- Samurai Warriors</a></li><li><a href="https://opstree.com/blog/2023/09/26/apache-cassandra-migration-last-episode-3x-to-4x-migration/" title="Apache Cassandra Migration Last Episode:  3x to 4x Migration">Apache Cassandra Migration Last Episode:  3x to 4x Migration</a></li><li><a href="https://opstree.com/blog/2023/08/22/apache-cassandra-migration-3-x-to-4-x-ep-2-dc-and-dr-setup/" title="Apache Cassandra Migration: 3.x to 4.x Ep: 2 DC and DR Setup">Apache Cassandra Migration: 3.x to 4.x Ep: 2 DC and DR Setup</a></li><li><a href="https://opstree.com/blog/2023/08/01/apache-cassandra-migration-3-x-to-4-x-episode-1-basics/" title="Apache Cassandra Migration: 3.x to 4.x Episode: 1 Basics">Apache Cassandra Migration: 3.x to 4.x Episode: 1 Basics</a></li><li><a href="https://opstree.com/blog/2023/03/28/aws-transit-gateway-a-saviour-for-your-connections/" title="AWS Transit Gateway &#8211; A Saviour for your Connections">AWS Transit Gateway &#8211; A Saviour for your Connections</a></li></ul></li><li><a href="https://opstree.com/blog/author/pksingh98/"> <img alt='1' src='https://secure.gravatar.com/avatar/d9d6a78042ce8765d86642451c799d1c?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/d9d6a78042ce8765d86642451c799d1c?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Prashant Kumar</strong></a><ul><li><a href="https://opstree.com/blog/2024/06/18/security-group-strategy-for-aws/" title="Security Group Strategy for AWS">Security Group Strategy for AWS</a></li><li><a href="https://opstree.com/blog/2021/05/04/event-monitoring-using-aws-cloudtrail/" title="Event Monitoring Using AWS CloudTrail">Event Monitoring Using AWS CloudTrail</a></li><li><a href="https://opstree.com/blog/2021/01/19/postgres-cis-benchmark/" title="Postgres &#8211; CIS Benchmark">Postgres &#8211; CIS Benchmark</a></li><li><a href="https://opstree.com/blog/2020/11/17/elastic-siem-an-event-tracking-feature/" title="Elastic SIEM &#8211; An Event Tracking Feature">Elastic SIEM &#8211; An Event Tracking Feature</a></li></ul></li><li><a href="https://opstree.com/blog/author/prakashjha9f558ddc78/"> <img alt='1' src='https://secure.gravatar.com/avatar/4a76128efbb889fd37d9a50f6911b42d?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/4a76128efbb889fd37d9a50f6911b42d?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Prakash Jha</strong></a><ul><li><a href="https://opstree.com/blog/2022/07/19/terraform-version-upgrade/" title="Terraform Version Upgrade">Terraform Version Upgrade</a></li><li><a href="https://opstree.com/blog/2021/12/14/ecs-rollback-with-jenkins-active-choice-parameter/" title="ECS rollback with Jenkins Active Choice Parameter">ECS rollback with Jenkins Active Choice Parameter</a></li><li><a href="https://opstree.com/blog/2021/03/09/codeherent-automatic-cloud-diagrams-powered-by-terraform/" title="Codeherent: Automatic Cloud Diagrams Powered by Terraform">Codeherent: Automatic Cloud Diagrams Powered by Terraform</a></li></ul></li><li><a href="https://opstree.com/blog/author/priyanshi0707/"> <img alt='1' src='https://secure.gravatar.com/avatar/863b5758f40b7e44062b3a48738d7d12?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/863b5758f40b7e44062b3a48738d7d12?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Priyanshi Chauhan</strong></a><ul><li><a href="https://opstree.com/blog/2023/05/02/simplify-your-kubernetes-deployments-with-argocd-and-gitops/" title="Simplify Your Kubernetes Deployments with ArgoCD and GitOps ">Simplify Your Kubernetes Deployments with ArgoCD and GitOps </a></li><li><a href="https://opstree.com/blog/2023/01/24/protected-efk-stack-setup-for-kubernetes/" title="Protected EFK Stack Setup for Kubernetes" aria-current="page">Protected EFK Stack Setup for Kubernetes</a></li></ul></li><li><a href="https://opstree.com/blog/author/rachitshrivastava93/"> <img alt='1' src='https://secure.gravatar.com/avatar/805127e762298d420057f395f41c5405?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/805127e762298d420057f395f41c5405?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>rachitshrivastava93</strong></a><ul><li><a href="https://opstree.com/blog/2020/12/08/dockerfile-hidden-secrets/" title="Hidden Secrets of Dockerfile">Hidden Secrets of Dockerfile</a></li></ul></li><li><a href="https://opstree.com/blog/author/rahuldubey973caad802/"> <img alt='1' src='https://secure.gravatar.com/avatar/62c7a13a50e1b8b5f7ddb0c01a53cabe?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/62c7a13a50e1b8b5f7ddb0c01a53cabe?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Rahul Dubey</strong></a><ul><li><a href="https://opstree.com/blog/2019/02/14/my-stint-with-runc-vulnerability/" title="My stint with Runc vulnerability">My stint with Runc vulnerability</a></li></ul></li><li><a href="https://opstree.com/blog/author/rahulmeena1509/"> <img alt='1' src='https://secure.gravatar.com/avatar/78e01c10d9b6a2f7f1d1e98d105333d3?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/78e01c10d9b6a2f7f1d1e98d105333d3?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>rahulmeena1509</strong></a><ul><li><a href="https://opstree.com/blog/2021/10/26/aws-lambda-heres-everything-you-need-to-know/" title="AWS LAMBDA &#8211; Here&#8217;s Everything You Need to Know!">AWS LAMBDA &#8211; Here&#8217;s Everything You Need to Know!</a></li></ul></li><li><a href="https://opstree.com/blog/author/rahulsachdeva2/"> <img alt='1' src='https://secure.gravatar.com/avatar/c55ecb803e5d1e2214d346d5b57efb7e?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/c55ecb803e5d1e2214d346d5b57efb7e?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>rahulsachdeva2</strong></a><ul><li><a href="https://opstree.com/blog/2021/07/13/opengrok-setup-and-features/" title="OpenGrok Setup and Features">OpenGrok Setup and Features</a></li></ul></li><li><a href="https://opstree.com/blog/author/raiaastha/"> <img alt='1' src='https://secure.gravatar.com/avatar/011b7cb3f137abffe2148747a03b0f6b?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/011b7cb3f137abffe2148747a03b0f6b?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>raiaastha</strong></a><ul><li><a href="https://opstree.com/blog/2021/02/17/resolution-of-apt-key-error/" title="Resolution of Apt-Key Error">Resolution of Apt-Key Error</a></li></ul></li><li><a href="https://opstree.com/blog/author/rajatchauhana9c6f9ab4f/"> <img alt='1' src='https://secure.gravatar.com/avatar/5c4f48cea16c5dcc822b89adda646da3?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/5c4f48cea16c5dcc822b89adda646da3?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Rajat Chauhan</strong></a><ul><li><a href="https://opstree.com/blog/2021/11/16/aws-secret-manager/" title="AWS SECRET MANAGER">AWS SECRET MANAGER</a></li></ul></li><li><a href="https://opstree.com/blog/author/rajatravi85aee98825/"> <img alt='1' src='https://secure.gravatar.com/avatar/a94bad6961f1ee6f726a60e84c362b86?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/a94bad6961f1ee6f726a60e84c362b86?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Rajat Ravi</strong></a><ul><li><a href="https://opstree.com/blog/2019/07/09/postfix-email-server-integration-with-ses/" title="Postfix Email Server integration with SES">Postfix Email Server integration with SES</a></li></ul></li><li><a href="https://opstree.com/blog/author/rajneesh-yadav/"> <img alt='1' src='https://secure.gravatar.com/avatar/e1c913135c565d743c1f71a8ac2a0e97?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/e1c913135c565d743c1f71a8ac2a0e97?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Rajneesh Yadav</strong></a><ul><li><a href="https://opstree.com/blog/2024/12/24/end-to-end-rag-solution-with-aws-bedrock-and-langchai/" title="End-to-End RAG Solution with AWS Bedrock and LangChain">End-to-End RAG Solution with AWS Bedrock and LangChain</a></li></ul></li><li><a href="https://opstree.com/blog/author/rakeshkumarkhetwal/"> <img alt='1' src='https://secure.gravatar.com/avatar/8f48c2e4621737c8fe7887df35d998e8?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/8f48c2e4621737c8fe7887df35d998e8?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>rakeshkumarkhetwal</strong></a><ul><li><a href="https://opstree.com/blog/2022/01/18/host-based-intrusion-detection-using-ossec/" title="HOST-BASED INTRUSION DETECTION USING OSSEC">HOST-BASED INTRUSION DETECTION USING OSSEC</a></li><li><a href="https://opstree.com/blog/2021/10/12/basics-of-amazon-route-53-part-1/" title="Basics of Amazon Route 53 [Part -1]">Basics of Amazon Route 53 [Part -1]</a></li></ul></li><li><a href="https://opstree.com/blog/author/ramneek-kaur/"> <img alt='1' src='https://secure.gravatar.com/avatar/106a7f9123ff438cbb028b1bec13cc83?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/106a7f9123ff438cbb028b1bec13cc83?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ramneek Kaur</strong></a><ul><li><a href="https://opstree.com/blog/2024/10/22/time-travel-queries-in-apache-hudi/" title="Exploring Time Travel Queries in Apache Hudi">Exploring Time Travel Queries in Apache Hudi</a></li></ul></li><li><a href="https://opstree.com/blog/author/ravindersingh/"> <img alt='1' src='https://secure.gravatar.com/avatar/00f6b29b69d247c619d3af78c2a91206?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/00f6b29b69d247c619d3af78c2a91206?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ravinder Singh</strong></a><ul><li><a href="https://opstree.com/blog/2024/05/07/azure-synapse-social-media-analytics-solution/" title="Azure Synapse Social Media Analytics Solution">Azure Synapse Social Media Analytics Solution</a></li></ul></li><li><a href="https://opstree.com/blog/author/ravindrakumarot/"> <img alt='1' src='https://secure.gravatar.com/avatar/8e99810b0581da008ad0ce016947b873?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/8e99810b0581da008ad0ce016947b873?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ravindra Kumar</strong></a><ul><li><a href="https://opstree.com/blog/2023/10/17/demystifying-oracle-cloud-infrastructure-oci-a-comprehensive-introduction-and-architecture-overview/" title="Demystifying Oracle Cloud Infrastructure (OCI): A Comprehensive Introduction and Architecture Overview">Demystifying Oracle Cloud Infrastructure (OCI): A Comprehensive Introduction and Architecture Overview</a></li></ul></li><li><a href="https://opstree.com/blog/author/reenanain/"> <img alt='1' src='https://secure.gravatar.com/avatar/10af7677b693089b8c325c597d5313a1?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/10af7677b693089b8c325c597d5313a1?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>reenanain</strong></a><ul><li><a href="https://opstree.com/blog/2022/07/05/learn-the-hacks-of-internal-load-balancing-in-aws-through-cross-region-vpc-peering/" title="Cross Region Internal Load Balancing  in AWS with VPC Peering">Cross Region Internal Load Balancing  in AWS with VPC Peering</a></li></ul></li><li><a href="https://opstree.com/blog/author/rishabhsharma7/"> <img alt='1' src='https://secure.gravatar.com/avatar/0a5f28bcdb0c399f3aec5bb69516c781?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/0a5f28bcdb0c399f3aec5bb69516c781?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Rishabh Sharma</strong></a><ul><li><a href="https://opstree.com/blog/2023/01/17/on-premise-setup-of-kubernetes-cluster-components-offline-mode-part-2/" title="On-Premise Setup of Kubernetes Cluster Components (Offline Mode) &#8211; PART 2">On-Premise Setup of Kubernetes Cluster Components (Offline Mode) &#8211; PART 2</a></li><li><a href="https://opstree.com/blog/2022/10/25/on-premise-setup-of-kubernetes-cluster-using-kubespray-offline-mode-part-1/" title="On-Premise Setup of Kubernetes Cluster using KubeSpray (Offline Mode) &#8211; PART 1">On-Premise Setup of Kubernetes Cluster using KubeSpray (Offline Mode) &#8211; PART 1</a></li><li><a href="https://opstree.com/blog/2022/07/26/how-to-fix-the-dpkg-lock-file-error-in-packer/" title="How to fix the dpkg lock file error in Packer?">How to fix the dpkg lock file error in Packer?</a></li></ul></li><li><a href="https://opstree.com/blog/author/rishops/"> <img alt='1' src='https://secure.gravatar.com/avatar/79647fd6b8d3c2165854b9bf7ec5f20a?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/79647fd6b8d3c2165854b9bf7ec5f20a?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Rishabh Bohra</strong></a><ul><li><a href="https://opstree.com/blog/2023/12/26/agentless-monitoring-integrating-supabase-metrics-with-grafana-cloud/" title="Agentless Monitoring: Integrating Supabase Metrics with Grafana Cloud">Agentless Monitoring: Integrating Supabase Metrics with Grafana Cloud</a></li></ul></li><li><a href="https://opstree.com/blog/author/riteshkumar5a60e2df5a/"> <img alt='1' src='https://secure.gravatar.com/avatar/946897cabb97a22b4ac56f3b469f5440?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/946897cabb97a22b4ac56f3b469f5440?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>riteshkumar5a60e2df5a</strong></a><ul><li><a href="https://opstree.com/blog/2024/04/30/iac-security-analysis-checkov-vs-tfsec-vs-terrascan-a-comparative-evaluation/" title="IaC Security Analysis: Checkov vs. tfsec vs. Terrascan &#8211; A Comparative Evaluation">IaC Security Analysis: Checkov vs. tfsec vs. Terrascan &#8211; A Comparative Evaluation</a></li></ul></li><li><a href="https://opstree.com/blog/author/roshanchandekar/"> <img alt='1' src='https://secure.gravatar.com/avatar/a612affe23a5cfc4eb55bf0f627d3403?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/a612affe23a5cfc4eb55bf0f627d3403?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Roshan Chandekar</strong></a><ul><li><a href="https://opstree.com/blog/2023/11/28/amazon-ecr-container-images-across-accounts-or-regions/" title="Amazon ECR Container Images Across Accounts or Regions">Amazon ECR Container Images Across Accounts or Regions</a></li><li><a href="https://opstree.com/blog/2022/05/17/learn-the-importance-of-namespace-quota-limits/" title="Learn the Importance of Namespace, Quota &amp; Limits">Learn the Importance of Namespace, Quota &amp; Limits</a></li></ul></li><li><a href="https://opstree.com/blog/author/ruchitavarma1/"> <img alt='1' src='https://secure.gravatar.com/avatar/7981795e60a1a659858c4b1691b47a2e?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/7981795e60a1a659858c4b1691b47a2e?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ruchita Varma</strong></a><ul><li><a href="https://opstree.com/blog/2022/05/17/3-best-tools-to-manage-your-kubernetes-cluster/" title="3 Best Tools to Manage Your Kubernetes Cluster!">3 Best Tools to Manage Your Kubernetes Cluster!</a></li><li><a href="https://opstree.com/blog/2022/04/20/when-not-to-think-of-canary-deployment/" title="When not to think of Canary Deployment?">When not to think of Canary Deployment?</a></li><li><a href="https://opstree.com/blog/2022/04/19/canary-vs-blue-green-deployment-which-one-should-you-choose/" title="Canary vs Blue-Green Deployment- Which one should you choose?">Canary vs Blue-Green Deployment- Which one should you choose?</a></li><li><a href="https://opstree.com/blog/2022/04/18/an-introduction-to-istio-service-mesh-its-architecture/" title="An Introduction to ISTIO Service Mesh &amp; its Architecture!">An Introduction to ISTIO Service Mesh &amp; its Architecture!</a></li><li><a href="https://opstree.com/blog/2022/04/14/the-benefits-of-deploying-a-service-mesh-istio/" title="The Benefits of Deploying a Service Mesh ISTIO!">The Benefits of Deploying a Service Mesh ISTIO!</a></li></ul></li><li><a href="https://opstree.com/blog/author/ruchitavarmab665988368/"> <img alt='1' src='https://secure.gravatar.com/avatar/25616017e893cac936f0233ab8b3be58?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/25616017e893cac936f0233ab8b3be58?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Ruchita Varma</strong></a><ul><li><a href="https://opstree.com/blog/2024/05/16/how-to-build-a-developer-metrics-dashboard/" title="How to Build a Developer Metrics Dashboard?">How to Build a Developer Metrics Dashboard?</a></li><li><a href="https://opstree.com/blog/2024/05/01/unraveling-the-spectrum-of-cloud-services/" title="Unraveling the Spectrum of Cloud Services">Unraveling the Spectrum of Cloud Services</a></li><li><a href="https://opstree.com/blog/2024/04/25/cloud-or-on-premise-it-infrastructure-whats-right-for-you/" title="Cloud or On-Premise IT Infrastructure: What&#8217;s Right for You?">Cloud or On-Premise IT Infrastructure: What&#8217;s Right for You?</a></li><li><a href="https://opstree.com/blog/2024/04/12/strategies-for-monitoring-cloud-based-data-processing/" title="Strategies for Monitoring Cloud-Based Data Processing">Strategies for Monitoring Cloud-Based Data Processing</a></li><li><a href="https://opstree.com/blog/2024/04/04/unveiling-cloud-vulnerabilities-top-3-security-concerns/" title="Unveiling Cloud Vulnerabilities: Top 3 Security Concerns">Unveiling Cloud Vulnerabilities: Top 3 Security Concerns</a></li></ul></li><li><a href="https://opstree.com/blog/author/sajaldevops/"> <img alt='1' src='https://secure.gravatar.com/avatar/0955d57877e8b6de39ab59e768094f4f?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/0955d57877e8b6de39ab59e768094f4f?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Sajal Jain</strong></a><ul><li><a href="https://opstree.com/blog/2019/10/29/redis-cluster-setup-sharding-and-failover-testing/" title="Redis Cluster: Setup, Sharding and Failover Testing">Redis Cluster: Setup, Sharding and Failover Testing</a></li><li><a href="https://opstree.com/blog/2019/10/22/redis-cluster-architecture-replication-sharding-and-failover/" title="Redis Cluster: Architecture, Replication, Sharding and Failover">Redis Cluster: Architecture, Replication, Sharding and Failover</a></li><li><a href="https://opstree.com/blog/2019/03/26/stay-away-replication-lag/" title="Stay Away Replication Lag !">Stay Away Replication Lag !</a></li></ul></li><li><a href="https://opstree.com/blog/author/sandeep7c51ad81ba/"> <img alt='1' src='https://secure.gravatar.com/avatar/28f3be900f701b6b62b9f6db5876638c?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/28f3be900f701b6b62b9f6db5876638c?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Sandeep Rawat</strong></a><ul><li><a href="https://opstree.com/blog/2017/02/03/gitolite/" title="Gitolite">Gitolite</a></li><li><a href="https://opstree.com/blog/2016/08/08/stunnel-a-proxy-to-ship-the-log-on-ssl/" title="Stunnel a Proxy to ship the log on SSL">Stunnel a Proxy to ship the log on SSL</a></li><li><a href="https://opstree.com/blog/2016/03/03/setup-of-nginx-vhost/" title="Setup of Nginx Vhost">Setup of Nginx Vhost</a></li><li><a href="https://opstree.com/blog/2016/02/09/jgit-flow-maven-plugin-to-release-java-application/" title="jgit-flow maven plugin to Release Java Application">jgit-flow maven plugin to Release Java Application</a></li><li><a href="https://opstree.com/blog/2015/01/13/kernel-based-virtualization-machine/" title="Kernel-based Virtualization Machine">Kernel-based Virtualization Machine</a></li></ul></li><li><a href="https://opstree.com/blog/author/sanket16494/"> <img alt='1' src='https://secure.gravatar.com/avatar/2f3d2012d72d96eded4f3c798fd2e351?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/2f3d2012d72d96eded4f3c798fd2e351?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Sanket Gupta</strong></a><ul><li><a href="https://opstree.com/blog/2021/01/05/elasticsearch-backup-and-restore-in-production/" title="Elasticsearch Backup and Restore in Production">Elasticsearch Backup and Restore in Production</a></li></ul></li><li><a href="https://opstree.com/blog/author/sanyamkalra1709/"> <img alt='1' src='https://secure.gravatar.com/avatar/f8238744260782bcf672094eaea4bb5b?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/f8238744260782bcf672094eaea4bb5b?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>sanyamkalra1709</strong></a><ul><li><a href="https://opstree.com/blog/2023/03/14/how-to-fix-a-corrupted-gui-after-downgrading-python-on-ubuntu/" title="How to Fix a Corrupted GUI after Downgrading Python on Ubuntu?">How to Fix a Corrupted GUI after Downgrading Python on Ubuntu?</a></li><li><a href="https://opstree.com/blog/2023/02/07/checkov-a-must-tool-for-infra-ci/" title="Checkov a Must Tool for Infra CI">Checkov a Must Tool for Infra CI</a></li></ul></li><li><a href="https://opstree.com/blog/author/saurabhvajpayee/"> <img alt='1' src='https://secure.gravatar.com/avatar/8644dcabef77ae5002b073cfbe324524?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/8644dcabef77ae5002b073cfbe324524?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>saurabhvajpayee</strong></a><ul><li><a href="https://opstree.com/blog/2016/04/21/kitchen-chefs-diagnosis-center/" title="Kitchen Chef&#8217;s diagnosis center">Kitchen Chef&#8217;s diagnosis center</a></li><li><a href="https://opstree.com/blog/2016/04/14/chef-kitchen-do-it-simply/" title="Chef-Kitchen Do it simply..">Chef-Kitchen Do it simply..</a></li><li><a href="https://opstree.com/blog/2016/04/07/chef-cookbooks-walls-of-chef-house/" title="Chef-Cookbooks Walls of chef-house..">Chef-Cookbooks Walls of chef-house..</a></li><li><a href="https://opstree.com/blog/2016/03/31/chef-cookbooks-roast-it-perfectly/" title="Chef-Cookbooks Roast it perfectly..">Chef-Cookbooks Roast it perfectly..</a></li><li><a href="https://opstree.com/blog/2016/03/24/chef-recipes-bake-it-calmly/" title="Chef-Recipes Bake it calmly..">Chef-Recipes Bake it calmly..</a></li></ul></li><li><a href="https://opstree.com/blog/author/shankara9edfeda38/"> <img alt='1' src='https://secure.gravatar.com/avatar/2ed8dbe2929012cf931fe295d5d9e6e1?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/2ed8dbe2929012cf931fe295d5d9e6e1?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Shankar Jha</strong></a><ul><li><a href="https://opstree.com/blog/2023/01/03/2022-a-year-of-incredible-firsts/" title="2022 &#8211; A year of Incredible Firsts!">2022 &#8211; A year of Incredible Firsts!</a></li><li><a href="https://opstree.com/blog/2020/12/01/opstree-opstree-labs-buildpiper-our-short-story/" title="OpsTree, OpsTree Labs &amp;  BuildPiper: Our Short Story…">OpsTree, OpsTree Labs &amp;  BuildPiper: Our Short Story…</a></li></ul></li><li><a href="https://opstree.com/blog/author/shatrujeetsah/"> <img alt='1' src='https://secure.gravatar.com/avatar/0af51132b328357c40b71312305277e8?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/0af51132b328357c40b71312305277e8?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>shatrujeetsah</strong></a><ul><li><a href="https://opstree.com/blog/2019/11/12/perfect-spot-instances-imperfections-part-ii/" title="Perfect Spot Instance&#8217;s Imperfections | part-II">Perfect Spot Instance&#8217;s Imperfections | part-II</a></li><li><a href="https://opstree.com/blog/2019/11/05/perfect-spot-instances-imperfections-part-i/" title="Perfect Spot Instance&#8217;s Imperfections | part-I">Perfect Spot Instance&#8217;s Imperfections | part-I</a></li></ul></li><li><a href="https://opstree.com/blog/author/shitunjay/"> <img alt='1' src='https://secure.gravatar.com/avatar/c24fd59129beee3dd4b59031d8783bda?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/c24fd59129beee3dd4b59031d8783bda?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>shitunjay</strong></a><ul><li><a href="https://opstree.com/blog/2023/02/21/split-tunneling-using-openvpn/" title="Split Tunneling Using OpenVPN">Split Tunneling Using OpenVPN</a></li><li><a href="https://opstree.com/blog/2022/11/15/active-active-infrastructure-using-terraform-and-jenkins-on-microsoft-azure/" title="Active-Active Infrastructure using Terraform and Jenkins on Microsoft Azure">Active-Active Infrastructure using Terraform and Jenkins on Microsoft Azure</a></li></ul></li><li><a href="https://opstree.com/blog/author/shristigupta/"> <img alt='1' src='https://secure.gravatar.com/avatar/157076a0cd3c03b3baebcd412dc42d99?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/157076a0cd3c03b3baebcd412dc42d99?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Shristi Gupta</strong></a><ul><li><a href="https://opstree.com/blog/2024/08/13/building-and-managing-production-ready-apache-airflow/" title="Building and Managing Production-Ready Apache Airflow: From Setup to Troubleshooting">Building and Managing Production-Ready Apache Airflow: From Setup to Troubleshooting</a></li></ul></li><li><a href="https://opstree.com/blog/author/shristigupta16/"> <img alt='1' src='https://secure.gravatar.com/avatar/3417b20dd616287f15c357619d4b4e70?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/3417b20dd616287f15c357619d4b4e70?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Shristi Gupta</strong></a><ul><li><a href="https://opstree.com/blog/2024/12/17/stream-postgresql-data-to-s3-via-kafka-using-jdbc-and-s3-sink-connectors-part-1/" title="Stream PostgreSQL Data to S3 via Kafka Using JDBC and S3 Sink Connectors : Part 1">Stream PostgreSQL Data to S3 via Kafka Using JDBC and S3 Sink Connectors : Part 1</a></li><li><a href="https://opstree.com/blog/2024/05/14/deploying-a-production-ready-kafka-cluster-on-kubernetes-with-strimzi/" title="Deploying a Production-Ready Kafka Cluster on Kubernetes with Strimzi">Deploying a Production-Ready Kafka Cluster on Kubernetes with Strimzi</a></li><li><a href="https://opstree.com/blog/2023/12/12/kernel-patching-with-the-help-of-loop-script/" title="Kernel Patching with the help of Loop Script">Kernel Patching with the help of Loop Script</a></li></ul></li><li><a href="https://opstree.com/blog/author/shubh11994/"> <img alt='1' src='https://secure.gravatar.com/avatar/b9f8cb13e5c0a0324e0f70c78f1f521f?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/b9f8cb13e5c0a0324e0f70c78f1f521f?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Shubh Joshi</strong></a><ul><li><a href="https://opstree.com/blog/2019/10/01/tuning-of-elasticsearch-cluster/" title="Tuning Of ElasticSearch Cluster">Tuning Of ElasticSearch Cluster</a></li></ul></li><li><a href="https://opstree.com/blog/author/shubham192001/"> <img alt='1' src='https://secure.gravatar.com/avatar/1d91666135d53a9bbcc5db1e5d3d4527?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/1d91666135d53a9bbcc5db1e5d3d4527?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>shubham192001</strong></a><ul><li><a href="https://opstree.com/blog/2024/09/24/extending-k8s-api/" title="Extending K8S API">Extending K8S API</a></li></ul></li><li><a href="https://opstree.com/blog/author/shubhamdhopate/"> <img alt='1' src='https://secure.gravatar.com/avatar/9baf5c5e4ee3ce02a0456b772fc23887?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/9baf5c5e4ee3ce02a0456b772fc23887?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>shubhamdhopate</strong></a><ul><li><a href="https://opstree.com/blog/2023/04/11/lambda-function-trigger-enabled-using-code-pipeline/" title="Lambda Function Trigger Enabled Using Code Pipeline.">Lambda Function Trigger Enabled Using Code Pipeline.</a></li></ul></li><li><a href="https://opstree.com/blog/author/shubhamdiwane/"> <img alt='1' src='https://secure.gravatar.com/avatar/d2ddc54afd00946a7c4038695f867312?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/d2ddc54afd00946a7c4038695f867312?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Shubham Diwane</strong></a><ul><li><a href="https://opstree.com/blog/2023/09/12/opensearch-alert-integration-with-sns/" title="OpenSearch Alert Integration with SNS">OpenSearch Alert Integration with SNS</a></li></ul></li><li><a href="https://opstree.com/blog/author/shubhamsahu26/"> <img alt='1' src='https://secure.gravatar.com/avatar/b5a041243155099635d23857eb96a850?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/b5a041243155099635d23857eb96a850?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>shubhamsahu26</strong></a><ul><li><a href="https://opstree.com/blog/2023/04/04/fossa-audit-grade-open-source-dependency-protection/" title="FOSSA: Audit-Grade Open Source Dependency Protection">FOSSA: Audit-Grade Open Source Dependency Protection</a></li></ul></li><li><a href="https://opstree.com/blog/author/shwetatyagiot/"> <img alt='1' src='https://secure.gravatar.com/avatar/5784dc0224052cdc83db53ada62be46d?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/5784dc0224052cdc83db53ada62be46d?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>shwetatyagiot</strong></a><ul><li><a href="https://opstree.com/blog/2022/04/05/know-how-to-use-velero-to-backup-and-migrate-kubernetes-resources-and-persistent-volumes/" title="Know How to Use Velero to Backup and Migrate Kubernetes Resources and Persistent Volumes">Know How to Use Velero to Backup and Migrate Kubernetes Resources and Persistent Volumes</a></li><li><a href="https://opstree.com/blog/2020/07/28/features-of-awx/" title="Features of AWX">Features of AWX</a></li></ul></li><li><a href="https://opstree.com/blog/author/sohandogra/"> <img alt='1' src='https://secure.gravatar.com/avatar/c0c3184193a406806e00d2d1542cc77c?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/c0c3184193a406806e00d2d1542cc77c?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Sohan Dogra</strong></a><ul><li><a href="https://opstree.com/blog/2022/11/22/pod-priority-priority-classamp-preemption/" title="Pod Priority, Priority Class, and Preemption">Pod Priority, Priority Class, and Preemption</a></li><li><a href="https://opstree.com/blog/2022/09/27/securing-k8s-traffic-with-cert-manager-amp-lets-encrypt/" title="Securing Kubernetes Traffic with Cert-Manager &amp; Lets Encrypt ">Securing Kubernetes Traffic with Cert-Manager &amp; Lets Encrypt </a></li></ul></li><li><a href="https://opstree.com/blog/author/srishtiaggarwalb6265a1943/"> <img alt='1' src='https://secure.gravatar.com/avatar/ac91beca528a1f9fe1856878c8e331d2?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/ac91beca528a1f9fe1856878c8e331d2?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Srishti Aggarwal</strong></a><ul><li><a href="https://opstree.com/blog/2020/08/25/how-to-setup-jenkins-in-just-few-minutes/" title="How to Setup Jenkins in a few minutes!">How to Setup Jenkins in a few minutes!</a></li></ul></li><li><a href="https://opstree.com/blog/author/subhabratasarkar/"> <img alt='1' src='https://secure.gravatar.com/avatar/a875912e3b6c2569062f3bae491d8780?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/a875912e3b6c2569062f3bae491d8780?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Subhabrata Sarkar</strong></a><ul><li><a href="https://opstree.com/blog/2022/03/15/know-how-to-access-s3-bucket-without-iam-roles-and-use-cases/" title="Know How to Access S3 Bucket without IAM Roles and Use Cases">Know How to Access S3 Bucket without IAM Roles and Use Cases</a></li><li><a href="https://opstree.com/blog/2022/02/08/learn-the-hacks-for-running-custom-scripts-at-spot-termination/" title="Learn the Hacks for Running Custom Scripts at Spot Termination">Learn the Hacks for Running Custom Scripts at Spot Termination</a></li></ul></li><li><a href="https://opstree.com/blog/author/sudiptsharma/"> <img alt='1' src='https://secure.gravatar.com/avatar/fd2ac0bca382947675bb0ab584cf9350?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/fd2ac0bca382947675bb0ab584cf9350?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Sudipt Sharma</strong></a><ul><li><a href="https://opstree.com/blog/2020/05/19/terraforming-the-better-way-part-i/" title="Terraforming The Better Way: Part-I">Terraforming The Better Way: Part-I</a></li><li><a href="https://opstree.com/blog/2019/09/10/unix-file-tree-part-2/" title="Unix File Tree Part-2">Unix File Tree Part-2</a></li><li><a href="https://opstree.com/blog/2019/07/16/unix-file-tree-part-1/" title="Unix File Tree Part-1">Unix File Tree Part-1</a></li><li><a href="https://opstree.com/blog/2018/12/26/git-submodule/" title="Git-Submodule">Git-Submodule</a></li><li><a href="https://opstree.com/blog/2018/12/03/gitlab-ci-with-nexus/" title="Gitlab-CI with Nexus">Gitlab-CI with Nexus</a></li></ul></li><li><a href="https://opstree.com/blog/author/sumitksuman/"> <img alt='1' src='https://secure.gravatar.com/avatar/616166df6468a20bfc93302a93195318?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/616166df6468a20bfc93302a93195318?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>sumitksuman</strong></a><ul><li><a href="https://opstree.com/blog/2019/12/24/how-to-test-ansible-playbook-role-using-molecule-with-docker/" title="How to test Ansible playbook/role using Molecules with Docker">How to test Ansible playbook/role using Molecules with Docker</a></li></ul></li><li><a href="https://opstree.com/blog/author/sunil2527/"> <img alt='1' src='https://secure.gravatar.com/avatar/640dd792b39fcb4d648b23e46f9ae7aa?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/640dd792b39fcb4d648b23e46f9ae7aa?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Sunil Kumar</strong></a><ul><li><a href="https://opstree.com/blog/2023/07/11/unlocking-debezium-exploring-the-fundamentals-of-real-time-change-data-capture-with-debezium-and-harnessing-its-power-in-docker-containers/" title="Unlocking Debezium: Exploring the Fundamentals of Real-Time Change Data Capture with Debezium and Harnessing its Power in Docker Containers">Unlocking Debezium: Exploring the Fundamentals of Real-Time Change Data Capture with Debezium and Harnessing its Power in Docker Containers</a></li></ul></li><li><a href="https://opstree.com/blog/author/sunilyadav23/"> <img alt='1' src='https://secure.gravatar.com/avatar/ec2d8257522390fc4cea73ea1c216ac6?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/ec2d8257522390fc4cea73ea1c216ac6?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>sunilyadav23</strong></a><ul><li><a href="https://opstree.com/blog/2019/05/14/ansible-dynamic-inventory-is-it-so-hard/" title="ANSIBLE DYNAMIC INVENTORY IS IT SO HARD?">ANSIBLE DYNAMIC INVENTORY IS IT SO HARD?</a></li></ul></li><li><a href="https://opstree.com/blog/author/surajknite/"> <img alt='1' src='https://secure.gravatar.com/avatar/0bacb6a194cab9325d36f70aebff63f4?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/0bacb6a194cab9325d36f70aebff63f4?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>surajknite</strong></a><ul><li><a href="https://opstree.com/blog/2022/04/19/a-savior-imperative-in-k8s/" title="A Savior &#8211; Imperative in K8s">A Savior &#8211; Imperative in K8s</a></li></ul></li><li><a href="https://opstree.com/blog/author/tarandeepsingh009/"> <img alt='1' src='https://secure.gravatar.com/avatar/a2f2eda633080e6892c96b5821275e75?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/a2f2eda633080e6892c96b5821275e75?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>tarandeepsingh009</strong></a><ul><li><a href="https://opstree.com/blog/2024/04/21/aws-direct-connect-a-gateway-to-dedicated-migration-solution/" title="AWS Direct Connect &#8211; A Gateway to Dedicated Migration Solution">AWS Direct Connect &#8211; A Gateway to Dedicated Migration Solution</a></li><li><a href="https://opstree.com/blog/2024/01/09/step-by-step-guide-to-setup-istio/" title="Step-by-Step Guide to Setup Istio">Step-by-Step Guide to Setup Istio</a></li><li><a href="https://opstree.com/blog/2023/06/06/jenkins-job-creation-using-multibranch-job-dsl/" title="Jenkins Job Creation using Multibranch Job DSL">Jenkins Job Creation using Multibranch Job DSL</a></li></ul></li><li><a href="https://opstree.com/blog/author/tejas1203/"> <img alt='1' src='https://secure.gravatar.com/avatar/28ca4795451649e32c4bfbdb41afdf68?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/28ca4795451649e32c4bfbdb41afdf68?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>tejas1203</strong></a><ul><li><a href="https://opstree.com/blog/2019/07/23/mysql-monitoring/" title="MySQL Monitoring">MySQL Monitoring</a></li></ul></li><li><a href="https://opstree.com/blog/author/tushar-panthari/"> <img alt='1' src='https://secure.gravatar.com/avatar/d050bc7034fa9b055cf94d18401ac876?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/d050bc7034fa9b055cf94d18401ac876?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Tushar Panthari</strong></a><ul><li><a href="https://opstree.com/blog/2024/12/27/can-cloud-data-be-hacked/" title="Can Cloud Data Be Hacked">Can Cloud Data Be Hacked</a></li><li><a href="https://opstree.com/blog/2024/12/23/the-role-of-ai-in-healthcare-applications-and-use-cases/" title="The Role of AI in Healthcare: Applications and Use Cases">The Role of AI in Healthcare: Applications and Use Cases</a></li><li><a href="https://opstree.com/blog/2024/12/18/top-10-lessons-learned-from-failed-cloud-migrations-what-went-wrong/" title="Top 10 Lessons Learned from Failed Cloud Migrations: What Went Wrong?">Top 10 Lessons Learned from Failed Cloud Migrations: What Went Wrong?</a></li></ul></li><li><a href="https://opstree.com/blog/author/varshakanwar/"> <img alt='1' src='https://secure.gravatar.com/avatar/9e3bab49cc938df3b58b4f65946e1e03?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/9e3bab49cc938df3b58b4f65946e1e03?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>varshakanwar</strong></a><ul><li><a href="https://opstree.com/blog/2023/05/23/cloud-cost-estimation-with-infracost/" title="Cloud Cost Estimation with Infracost">Cloud Cost Estimation with Infracost</a></li></ul></li><li><a href="https://opstree.com/blog/author/vidhi-yadav/"> <img alt='1' src='https://secure.gravatar.com/avatar/c625bc7e626ea3dff6170c7db64f8848?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/c625bc7e626ea3dff6170c7db64f8848?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Vidhi Yadav</strong></a><ul><li><a href="https://opstree.com/blog/2024/11/19/restoring-a-backup-stored-in-s3-to-an-ec2-instance-using-xtrabackup/" title="Restoring a Backup Stored in S3 to an EC2 Instance Using XtraBackup">Restoring a Backup Stored in S3 to an EC2 Instance Using XtraBackup</a></li><li><a href="https://opstree.com/blog/2024/11/05/amazon-s3-security-essentials-protect-your-data-with-these-key-practices/" title="Amazon S3 Security Essentials: Protect Your Data with These Key Practices">Amazon S3 Security Essentials: Protect Your Data with These Key Practices</a></li><li><a href="https://opstree.com/blog/2024/10/29/setup-cross-cluster-replication-for-data-migration-in-elasticsearch/" title="Setup Cross Cluster Replication for Data migration in Elasticsearch">Setup Cross Cluster Replication for Data migration in Elasticsearch</a></li><li><a href="https://opstree.com/blog/2024/10/15/getting-started-with-streamlit-build-interactive-data-apps-in-python/" title="Getting Started with StreamLit: Build Interactive Data Apps in Python">Getting Started with StreamLit: Build Interactive Data Apps in Python</a></li></ul></li><li><a href="https://opstree.com/blog/author/vikashgautam93/"> <img alt='1' src='https://secure.gravatar.com/avatar/44198cf101f2ba8722e644b04582696d?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/44198cf101f2ba8722e644b04582696d?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>V!kash Gautam</strong></a><ul><li><a href="https://opstree.com/blog/2024/03/12/dependency-management-with-renovate-beyond-the-limits-of-dependabot/" title="Dependency Management with Renovate: Beyond the Limits of Dependabot">Dependency Management with Renovate: Beyond the Limits of Dependabot</a></li><li><a href="https://opstree.com/blog/2023/01/31/kubernetes-cri-container-runtime-interface/" title="Kubernetes CRI — Container Runtime Interface">Kubernetes CRI — Container Runtime Interface</a></li><li><a href="https://opstree.com/blog/2021/10/19/how-to-fix-error-ssl-certificate-verify-failed/" title="How to fix error &#8220;[SSL: CERTIFICATE_ VERIFY_FAILED] certificate verify failed&#8221; (_ssl.c:727)">How to fix error &#8220;[SSL: CERTIFICATE_ VERIFY_FAILED] certificate verify failed&#8221; (_ssl.c:727)</a></li><li><a href="https://opstree.com/blog/2021/06/22/enable-support-to-provision-gp3-volumes-in-storage-class/" title="Enable Support to Provision GP3 Volumes in Storage Class">Enable Support to Provision GP3 Volumes in Storage Class</a></li><li><a href="https://opstree.com/blog/2020/06/16/a-closer-look-at-coredns/" title="A Closer Look at coreDNS">A Closer Look at coreDNS</a></li></ul></li><li><a href="https://opstree.com/blog/author/vineetyadav97/"> <img alt='1' src='https://secure.gravatar.com/avatar/4eba9383472cbc85013db2b59f4eef40?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/4eba9383472cbc85013db2b59f4eef40?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>vineetyadav97</strong></a><ul><li><a href="https://opstree.com/blog/2022/05/10/alerting-through-azure-logic-apps/" title="Alerting Through Azure Logic Apps">Alerting Through Azure Logic Apps</a></li><li><a href="https://opstree.com/blog/2022/01/25/the-step-by-step-guide-to-connect-aws-with-azure/" title="The Step-By-Step Guide to Connect Aws with Azure">The Step-By-Step Guide to Connect Aws with Azure</a></li></ul></li><li><a href="https://opstree.com/blog/author/vishalraje216ca9a3b/"> <img alt='1' src='https://secure.gravatar.com/avatar/c65589a3da679207c37441c1abd14aa5?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/c65589a3da679207c37441c1abd14aa5?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Vishal Raj</strong></a><ul><li><a href="https://opstree.com/blog/2020/12/29/devsecops-diary-hipaa-compliance/" title="DevSecOps Diary | HIPAA Compliance">DevSecOps Diary | HIPAA Compliance</a></li><li><a href="https://opstree.com/blog/2020/10/13/kubernetes-diary-software-loadbalancer/" title="Kubernetes Diary &#8211; Software LoadBalancer">Kubernetes Diary &#8211; Software LoadBalancer</a></li></ul></li><li><a href="https://opstree.com/blog/author/vishalsaini1100/"> <img alt='1' src='https://secure.gravatar.com/avatar/9edf4d3d019ed05169f2f1b4a57f9b57?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/9edf4d3d019ed05169f2f1b4a57f9b57?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Vishal Singh Saini</strong></a><ul><li><a href="https://opstree.com/blog/2022/01/04/records-creation-in-azure-dns-from-aks-externaldns/" title="Records Creation in Azure DNS from AKS ExternalDNS">Records Creation in Azure DNS from AKS ExternalDNS</a></li></ul></li><li><a href="https://opstree.com/blog/author/vishantsharma171d48ffb0/"> <img alt='1' src='https://secure.gravatar.com/avatar/10eebfe74ac39267dddb4e6e6d6385a8?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/10eebfe74ac39267dddb4e6e6d6385a8?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Vishant Sharma</strong></a><ul><li><a href="https://opstree.com/blog/2022/05/31/azure-ha-kubernetes-monitoring-using-prometheus-and-thanos/" title="Azure HA Kubernetes Monitoring using Prometheus and&nbsp;Thanos">Azure HA Kubernetes Monitoring using Prometheus and&nbsp;Thanos</a></li><li><a href="https://opstree.com/blog/2020/06/02/sonarqube-custom-quality-profiles/" title="SonarQube Custom Quality Profiles">SonarQube Custom Quality Profiles</a></li><li><a href="https://opstree.com/blog/2019/09/04/jenkins-pipeline-global-shared-libraries/" title="Jenkins Pipeline Global Shared Libraries">Jenkins Pipeline Global Shared Libraries</a></li><li><a href="https://opstree.com/blog/2019/04/23/kafka-manager-on-kubernetes/" title="Kafka Manager On  Kubernetes">Kafka Manager On  Kubernetes</a></li></ul></li><li><a href="https://opstree.com/blog/author/vishnudass/"> <img alt='1' src='https://secure.gravatar.com/avatar/03d933ec00a9597d1ec0f006380fea69?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/03d933ec00a9597d1ec0f006380fea69?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>Vishnu dass</strong></a><ul><li><a href="https://opstree.com/blog/2024/11/29/how-to-activate-virtual-environment-in-python-vs-code/" title="How to Activate Virtual Environment in Python VS Code">How to Activate Virtual Environment in Python VS Code</a></li><li><a href="https://opstree.com/blog/2024/11/18/guide-to-implementing-gitops-with-argocd/" title="Implementing GitOps with ArgoCD">Implementing GitOps with ArgoCD</a></li><li><a href="https://opstree.com/blog/2024/11/08/modern-traffic-management-with-gateway-api-in-kubernetes/" title="Modern Traffic Management with Gateway API in Kubernetes">Modern Traffic Management with Gateway API in Kubernetes</a></li><li><a href="https://opstree.com/blog/2024/10/28/transforming-legacy-systems-common-pitfalls-and-best-practices/" title="Transforming Legacy Systems: Common Pitfalls and Best Practices">Transforming Legacy Systems: Common Pitfalls and Best Practices</a></li><li><a href="https://opstree.com/blog/2024/10/18/addressing-the-rise-of-cloud-security-threats-best-practices-for-2024/" title="Addressing the Rise of Cloud Security Threats: Best Practices for 2024">Addressing the Rise of Cloud Security Threats: Best Practices for 2024</a></li></ul></li><li><a href="https://opstree.com/blog/author/waliarohit/"> <img alt='1' src='https://secure.gravatar.com/avatar/b4f4c0021159bb09da421f1def3f5968?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/b4f4c0021159bb09da421f1def3f5968?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>waliarohit</strong></a><ul><li><a href="https://opstree.com/blog/2019/03/01/its-not-you-everytime-sometimes-issue-might-be-at-aws-end/" title="Its not you Everytime, sometimes issue might be at AWS End">Its not you Everytime, sometimes issue might be at AWS End</a></li></ul></li><li><a href="https://opstree.com/blog/author/yamankg/"> <img alt='1' src='https://secure.gravatar.com/avatar/be437daaf0a7b82d6fb7ce2539403377?s=48&#038;d=identicon&#038;r=g' srcset='https://secure.gravatar.com/avatar/be437daaf0a7b82d6fb7ce2539403377?s=96&#038;d=identicon&#038;r=g 2x' class='avatar avatar-48 photo' height='48' width='48' loading='lazy' decoding='async'/> <strong>yamankg</strong></a><ul><li><a href="https://opstree.com/blog/2022/08/02/how-to-optimize-tick-alert-flooding-issue/" title="TICK | Alert Flooding Issue and Optimization">TICK | Alert Flooding Issue and Optimization</a></li></ul></li></ul></section>	</aside><!-- .sidebar .widget-area -->

		</div><!-- .site-content -->

		<footer id="colophon" class="site-footer">
							<nav class="main-navigation" aria-label="Footer Primary Menu">
					<div class="menu-primary-container"><ul id="menu-primary-1" class="primary-menu"><li class="menu-item menu-item-type-custom menu-item-object-custom menu-item-6"><a href="/">Home</a></li>
<li class="menu-item menu-item-type-custom menu-item-object-custom menu-item-13367"><a target="_blank" rel="noopener" href="https://opstree.com/services/">Services</a></li>
<li class="menu-item menu-item-type-custom menu-item-object-custom menu-item-13370"><a target="_blank" rel="noopener" href="https://opstree.com/case-study/">Case Study</a></li>
<li class="menu-item menu-item-type-post_type menu-item-object-page menu-item-7"><a href="https://opstree.com/blog/contact/">Contact</a></li>
<li class="menu-item menu-item-type-custom menu-item-object-custom menu-item-7661"><a href="/blog/write-for-us">Sign Up</a></li>
</ul></div>				</nav><!-- .main-navigation -->
			
			
			<div class="site-info">
								<span class="site-title"><a href="https://opstree.com/blog/" rel="home">DEVOPS DONE RIGHT.</a></span>
								<a href="https://wordpress.org/" class="imprint">
					Proudly powered by WordPress				</a>
			</div><!-- .site-info -->
		</footer><!-- .site-footer -->
	</div><!-- .site-inner -->
</div><!-- .site -->

	<div style="display:none">
			<div class="grofile-hash-map-863b5758f40b7e44062b3a48738d7d12">
		</div>
		<div class="grofile-hash-map-74ec7b8287279a6270583a118cd955bb">
		</div>
		<div class="grofile-hash-map-dc5fdfa8616442612858a03b339b64a4">
		</div>
		<div class="grofile-hash-map-caac723e8296d6ed94200f570007c5e0">
		</div>
		<div class="grofile-hash-map-9162389b77fa83266a23a0cfcbb92f42">
		</div>
		<div class="grofile-hash-map-10d5fdde476e8c28d8a795f61ad83860">
		</div>
		<div class="grofile-hash-map-8c1208ea43dbf283bfaac6f98f7e6e85">
		</div>
		<div class="grofile-hash-map-141b41fb538f1b2041a516386ab36228">
		</div>
		<div class="grofile-hash-map-0acf3f2dace713d126b3d7635990fa05">
		</div>
		<div class="grofile-hash-map-7ac624c1bf1a4070b77e33adcce7d94d">
		</div>
		<div class="grofile-hash-map-3b4335a1c10d80dc0b055ffa9efec2b8">
		</div>
		<div class="grofile-hash-map-ff33b9331a3f363407ac324e9f27ae49">
		</div>
		<div class="grofile-hash-map-7c1b313fbc64fd0d9cee72ae144f4f54">
		</div>
		<div class="grofile-hash-map-6b9e080fbd8c889e2b08508fb409d3b9">
		</div>
		<div class="grofile-hash-map-7a1a23af94f0c8ba09065eecb29d9ee5">
		</div>
		<div class="grofile-hash-map-c729bf9ed69bf6f17a32942bcf9acb84">
		</div>
		<div class="grofile-hash-map-6057d7bd4387695e2d3073bbc923ab61">
		</div>
		<div class="grofile-hash-map-da2c9138c866fcf7ddd3ae3ce2443fd9">
		</div>
		<div class="grofile-hash-map-b9e6dd34c46e0d6b089ec7ca8791ef2e">
		</div>
		<div class="grofile-hash-map-2908543bd99fc328b6d529699e26a278">
		</div>
		<div class="grofile-hash-map-99448473581227daad79c944d962c803">
		</div>
		<div class="grofile-hash-map-fbcc7e692990c92cfcf322ddfb7b265c">
		</div>
		<div class="grofile-hash-map-9028b66d911935e2fbed4a0294d3ada9">
		</div>
		<div class="grofile-hash-map-766a44bc759223d22309683bc9a31feb">
		</div>
		<div class="grofile-hash-map-33cfa9a9d33737b64fe94c4f83ce2b42">
		</div>
		<div class="grofile-hash-map-30c2cccbf4940ff24b1455f7a59cd22d">
		</div>
		<div class="grofile-hash-map-c184b141f2e40340df029d6c35f656a6">
		</div>
		<div class="grofile-hash-map-2c1422d5980debe6087b30be86788e25">
		</div>
		<div class="grofile-hash-map-356475eba7b59791e71a311770a375e0">
		</div>
		<div class="grofile-hash-map-199104af8cf90735e366aa57c3b3ab60">
		</div>
		<div class="grofile-hash-map-63ced52303308706a109ace586153220">
		</div>
		<div class="grofile-hash-map-8a9a6eb3734ffd292280296bfd562979">
		</div>
		<div class="grofile-hash-map-fdadc27f343143301e06cab928173ad0">
		</div>
		<div class="grofile-hash-map-f237ebcccdd8f32f82383e357e9e2f55">
		</div>
		<div class="grofile-hash-map-e7b872b15b8dff69cf6318bb6879507e">
		</div>
		<div class="grofile-hash-map-6d376e8fbea2051c7c225a7e1bf6f970">
		</div>
		<div class="grofile-hash-map-3ab752dc7d4d142d06a1ee3e2f6e612e">
		</div>
		<div class="grofile-hash-map-f7421a43dcc20e5e5e038309503c1d4b">
		</div>
		<div class="grofile-hash-map-b59a47d72b3b1fc82953c41603eaef60">
		</div>
		<div class="grofile-hash-map-bdc3eb4ed619c855a08200424081468d">
		</div>
		<div class="grofile-hash-map-5095710cfb67ada0bfd7a327ff16b2c5">
		</div>
		<div class="grofile-hash-map-24416e151e3753a10588e3705fcea587">
		</div>
		<div class="grofile-hash-map-7b8705d543aae6638e1f12c79f06a717">
		</div>
		<div class="grofile-hash-map-5ad028a3b47c0d2c77d5d3a6cbe514fa">
		</div>
		<div class="grofile-hash-map-3e82dc4ff64b8b99aff476251dc479a0">
		</div>
		<div class="grofile-hash-map-d188a527a2f683eac8b1e2376277adad">
		</div>
		<div class="grofile-hash-map-ce9105efbdf9dfba248d42f8bd747b7e">
		</div>
		<div class="grofile-hash-map-33d43470ba1aeff4e119252c598d23d8">
		</div>
		<div class="grofile-hash-map-48b925bdba49ce4f97f672befa9b8cb9">
		</div>
		<div class="grofile-hash-map-27ae662365bff6aabba46f5c865f08f0">
		</div>
		<div class="grofile-hash-map-8a55e7767bddd65be95d232903fe2263">
		</div>
		<div class="grofile-hash-map-b8b39cd0befb9ceedde9410d70bfb6aa">
		</div>
		<div class="grofile-hash-map-52528ee3247b1e8a6a41b09f380878dc">
		</div>
		<div class="grofile-hash-map-d68c2d4484b3c7b62ab4f9cb255b4a8e">
		</div>
		<div class="grofile-hash-map-e61bf6d3f0cf884284ebdaf164be3c92">
		</div>
		<div class="grofile-hash-map-69e807be0373a03ddc373c87bf467b0e">
		</div>
		<div class="grofile-hash-map-840d9f0059bcfdc4f05d8776354c8bbe">
		</div>
		<div class="grofile-hash-map-11ecc2a8728c57219bba283e2692357f">
		</div>
		<div class="grofile-hash-map-002f52e52fc5093f167ecf3fcfa6dde9">
		</div>
		<div class="grofile-hash-map-5d366fea7c10c5d85a31bdd7b8d0d2f7">
		</div>
		<div class="grofile-hash-map-0bd97dadbc77e080da7ddf9b1be3beaa">
		</div>
		<div class="grofile-hash-map-52bbd6f88135c3618cc8606082332d57">
		</div>
		<div class="grofile-hash-map-f9d862e412ed12b6f0e4949a0a1ec94f">
		</div>
		<div class="grofile-hash-map-cba69c3b014ed34fc5d09548568e6cae">
		</div>
		<div class="grofile-hash-map-21fc0b696d91f1a67ba3633865874cd4">
		</div>
		<div class="grofile-hash-map-c2f318d919d6ff940b8c16a43ce2cfc3">
		</div>
		<div class="grofile-hash-map-2116a20a4b4cd4df0f188238b62e657c">
		</div>
		<div class="grofile-hash-map-d1831ddad74cccf6afb47f41927f5a5e">
		</div>
		<div class="grofile-hash-map-1f719975a3ed2c329af57a18ed4d2dfd">
		</div>
		<div class="grofile-hash-map-92ece5fc64fc62eff0dbd132ab870249">
		</div>
		<div class="grofile-hash-map-779408028e4d056a969b9a1f31225d1f">
		</div>
		<div class="grofile-hash-map-229b40997c93c7e27792fd3a1750edf5">
		</div>
		<div class="grofile-hash-map-2b22bec031e6b16e76dd67fdc6e6539b">
		</div>
		<div class="grofile-hash-map-0334176f8963b20189f67c0358dc9727">
		</div>
		<div class="grofile-hash-map-18b224464e7f2fd41570db4065561921">
		</div>
		<div class="grofile-hash-map-9ca482049807a0a99d56c395b9fd1731">
		</div>
		<div class="grofile-hash-map-38ae238198787288a3d5d882a155345b">
		</div>
		<div class="grofile-hash-map-243e2e3c66b39675e3b055ac5bffbf0e">
		</div>
		<div class="grofile-hash-map-3e7e4858de510f72484081d58b3a3f2d">
		</div>
		<div class="grofile-hash-map-68b794ac2d835b575ea988aff5f9de5f">
		</div>
		<div class="grofile-hash-map-d9d6a78042ce8765d86642451c799d1c">
		</div>
		<div class="grofile-hash-map-4a76128efbb889fd37d9a50f6911b42d">
		</div>
		<div class="grofile-hash-map-863b5758f40b7e44062b3a48738d7d12">
		</div>
		<div class="grofile-hash-map-805127e762298d420057f395f41c5405">
		</div>
		<div class="grofile-hash-map-62c7a13a50e1b8b5f7ddb0c01a53cabe">
		</div>
		<div class="grofile-hash-map-78e01c10d9b6a2f7f1d1e98d105333d3">
		</div>
		<div class="grofile-hash-map-c55ecb803e5d1e2214d346d5b57efb7e">
		</div>
		<div class="grofile-hash-map-011b7cb3f137abffe2148747a03b0f6b">
		</div>
		<div class="grofile-hash-map-5c4f48cea16c5dcc822b89adda646da3">
		</div>
		<div class="grofile-hash-map-a94bad6961f1ee6f726a60e84c362b86">
		</div>
		<div class="grofile-hash-map-e1c913135c565d743c1f71a8ac2a0e97">
		</div>
		<div class="grofile-hash-map-8f48c2e4621737c8fe7887df35d998e8">
		</div>
		<div class="grofile-hash-map-106a7f9123ff438cbb028b1bec13cc83">
		</div>
		<div class="grofile-hash-map-00f6b29b69d247c619d3af78c2a91206">
		</div>
		<div class="grofile-hash-map-8e99810b0581da008ad0ce016947b873">
		</div>
		<div class="grofile-hash-map-10af7677b693089b8c325c597d5313a1">
		</div>
		<div class="grofile-hash-map-0a5f28bcdb0c399f3aec5bb69516c781">
		</div>
		<div class="grofile-hash-map-79647fd6b8d3c2165854b9bf7ec5f20a">
		</div>
		<div class="grofile-hash-map-946897cabb97a22b4ac56f3b469f5440">
		</div>
		<div class="grofile-hash-map-a612affe23a5cfc4eb55bf0f627d3403">
		</div>
		<div class="grofile-hash-map-7981795e60a1a659858c4b1691b47a2e">
		</div>
		<div class="grofile-hash-map-25616017e893cac936f0233ab8b3be58">
		</div>
		<div class="grofile-hash-map-0955d57877e8b6de39ab59e768094f4f">
		</div>
		<div class="grofile-hash-map-28f3be900f701b6b62b9f6db5876638c">
		</div>
		<div class="grofile-hash-map-2f3d2012d72d96eded4f3c798fd2e351">
		</div>
		<div class="grofile-hash-map-f8238744260782bcf672094eaea4bb5b">
		</div>
		<div class="grofile-hash-map-8644dcabef77ae5002b073cfbe324524">
		</div>
		<div class="grofile-hash-map-2ed8dbe2929012cf931fe295d5d9e6e1">
		</div>
		<div class="grofile-hash-map-0af51132b328357c40b71312305277e8">
		</div>
		<div class="grofile-hash-map-c24fd59129beee3dd4b59031d8783bda">
		</div>
		<div class="grofile-hash-map-157076a0cd3c03b3baebcd412dc42d99">
		</div>
		<div class="grofile-hash-map-3417b20dd616287f15c357619d4b4e70">
		</div>
		<div class="grofile-hash-map-b9f8cb13e5c0a0324e0f70c78f1f521f">
		</div>
		<div class="grofile-hash-map-1d91666135d53a9bbcc5db1e5d3d4527">
		</div>
		<div class="grofile-hash-map-9baf5c5e4ee3ce02a0456b772fc23887">
		</div>
		<div class="grofile-hash-map-d2ddc54afd00946a7c4038695f867312">
		</div>
		<div class="grofile-hash-map-b5a041243155099635d23857eb96a850">
		</div>
		<div class="grofile-hash-map-5784dc0224052cdc83db53ada62be46d">
		</div>
		<div class="grofile-hash-map-c0c3184193a406806e00d2d1542cc77c">
		</div>
		<div class="grofile-hash-map-ac91beca528a1f9fe1856878c8e331d2">
		</div>
		<div class="grofile-hash-map-a875912e3b6c2569062f3bae491d8780">
		</div>
		<div class="grofile-hash-map-fd2ac0bca382947675bb0ab584cf9350">
		</div>
		<div class="grofile-hash-map-616166df6468a20bfc93302a93195318">
		</div>
		<div class="grofile-hash-map-640dd792b39fcb4d648b23e46f9ae7aa">
		</div>
		<div class="grofile-hash-map-ec2d8257522390fc4cea73ea1c216ac6">
		</div>
		<div class="grofile-hash-map-0bacb6a194cab9325d36f70aebff63f4">
		</div>
		<div class="grofile-hash-map-a2f2eda633080e6892c96b5821275e75">
		</div>
		<div class="grofile-hash-map-28ca4795451649e32c4bfbdb41afdf68">
		</div>
		<div class="grofile-hash-map-d050bc7034fa9b055cf94d18401ac876">
		</div>
		<div class="grofile-hash-map-9e3bab49cc938df3b58b4f65946e1e03">
		</div>
		<div class="grofile-hash-map-c625bc7e626ea3dff6170c7db64f8848">
		</div>
		<div class="grofile-hash-map-44198cf101f2ba8722e644b04582696d">
		</div>
		<div class="grofile-hash-map-4eba9383472cbc85013db2b59f4eef40">
		</div>
		<div class="grofile-hash-map-c65589a3da679207c37441c1abd14aa5">
		</div>
		<div class="grofile-hash-map-9edf4d3d019ed05169f2f1b4a57f9b57">
		</div>
		<div class="grofile-hash-map-10eebfe74ac39267dddb4e6e6d6385a8">
		</div>
		<div class="grofile-hash-map-03d933ec00a9597d1ec0f006380fea69">
		</div>
		<div class="grofile-hash-map-b4f4c0021159bb09da421f1def3f5968">
		</div>
		<div class="grofile-hash-map-be437daaf0a7b82d6fb7ce2539403377">
		</div>
		</div>
		
	<script type="text/javascript">
		window.WPCOM_sharing_counts = {"https:\/\/opstree.com\/blog\/2023\/01\/24\/protected-efk-stack-setup-for-kubernetes\/":12726};
	</script>
				<link rel='stylesheet' id='jetpack-gist-styling-css' href='https://github.githubassets.com/assets/gist-embed-9eb50564113f.css?ver=13.3.2' media='all' />
<style id='core-block-supports-inline-css'>
.wp-container-core-social-links-is-layout-1.wp-container-core-social-links-is-layout-1{justify-content:center;}
</style>
<script src="https://opstree.com/blog/wp-content/plugins/jetpack/jetpack_vendor/automattic/jetpack-image-cdn/dist/image-cdn.js?minify=false&amp;ver=132249e245926ae3e188" id="jetpack-photon-js"></script>
<script src="https://opstree.com/blog/wp-content/plugins/coblocks/dist/js/coblocks-animation.js?ver=3.1.7" id="coblocks-animation-js"></script>
<script src="https://opstree.com/blog/wp-content/plugins/coblocks/dist/js/vendors/tiny-swiper.js?ver=3.1.7" id="coblocks-tiny-swiper-js"></script>
<script id="coblocks-tinyswiper-initializer-js-extra">
var coblocksTinyswiper = {"carouselPrevButtonAriaLabel":"Previous","carouselNextButtonAriaLabel":"Next","sliderImageAriaLabel":"Image"};
</script>
<script src="https://opstree.com/blog/wp-content/plugins/coblocks/dist/js/coblocks-tinyswiper-initializer.js?ver=3.1.7" id="coblocks-tinyswiper-initializer-js"></script>
<script src="https://secure.gravatar.com/js/gprofiles.js?ver=202452" id="grofiles-cards-js"></script>
<script id="wpgroho-js-extra">
var WPGroHo = {"my_hash":""};
</script>
<script src="https://c0.wp.com/p/jetpack/13.3.2/modules/wpgroho.js" id="wpgroho-js"></script>
<script src="https://c0.wp.com/c/6.5.5/wp-includes/js/comment-reply.min.js" id="comment-reply-js" async data-wp-strategy="async"></script>
<script src="https://c0.wp.com/p/jetpack/13.3.2/_inc/build/likes/queuehandler.min.js" id="jetpack_likes_queuehandler-js"></script>
<script src="https://stats.wp.com/e-202452.js" id="jetpack-stats-js" data-wp-strategy="defer"></script>
<script id="jetpack-stats-js-after">
_stq = window._stq || [];
_stq.push([ "view", JSON.parse("{\"v\":\"ext\",\"blog\":\"231085182\",\"post\":\"12726\",\"tz\":\"5.5\",\"srv\":\"opstree.com\",\"j\":\"1:13.3.2\"}") ]);
_stq.push([ "clickTrackerInit", "231085182", "12726" ]);
</script>
<script src="https://opstree.com/blog/wp-content/plugins/coblocks/dist/js/coblocks-gist-script.js?ver=3.1.7" id="coblocks-gist-script-js"></script>
<script defer src="https://opstree.com/blog/wp-content/plugins/akismet/_inc/akismet-frontend.js?ver=1710780648" id="akismet-frontend-js"></script>
<script id="sharing-js-js-extra">
var sharing_js_options = {"lang":"en","counts":"1","is_stats_active":"1"};
</script>
<script src="https://c0.wp.com/p/jetpack/13.3.2/_inc/build/sharedaddy/sharing.min.js" id="sharing-js-js"></script>
<script id="sharing-js-js-after">
var windowOpen;
			( function () {
				function matches( el, sel ) {
					return !! (
						el.matches && el.matches( sel ) ||
						el.msMatchesSelector && el.msMatchesSelector( sel )
					);
				}

				document.body.addEventListener( 'click', function ( event ) {
					if ( ! event.target ) {
						return;
					}

					var el;
					if ( matches( event.target, 'a.share-twitter' ) ) {
						el = event.target;
					} else if ( event.target.parentNode && matches( event.target.parentNode, 'a.share-twitter' ) ) {
						el = event.target.parentNode;
					}

					if ( el ) {
						event.preventDefault();

						// If there's another sharing window open, close it.
						if ( typeof windowOpen !== 'undefined' ) {
							windowOpen.close();
						}
						windowOpen = window.open( el.getAttribute( 'href' ), 'wpcomtwitter', 'menubar=1,resizable=1,width=600,height=350' );
						return false;
					}
				} );
			} )();
var windowOpen;
			( function () {
				function matches( el, sel ) {
					return !! (
						el.matches && el.matches( sel ) ||
						el.msMatchesSelector && el.msMatchesSelector( sel )
					);
				}

				document.body.addEventListener( 'click', function ( event ) {
					if ( ! event.target ) {
						return;
					}

					var el;
					if ( matches( event.target, 'a.share-linkedin' ) ) {
						el = event.target;
					} else if ( event.target.parentNode && matches( event.target.parentNode, 'a.share-linkedin' ) ) {
						el = event.target.parentNode;
					}

					if ( el ) {
						event.preventDefault();

						// If there's another sharing window open, close it.
						if ( typeof windowOpen !== 'undefined' ) {
							windowOpen.close();
						}
						windowOpen = window.open( el.getAttribute( 'href' ), 'wpcomlinkedin', 'menubar=1,resizable=1,width=580,height=450' );
						return false;
					}
				} );
			} )();
var windowOpen;
			( function () {
				function matches( el, sel ) {
					return !! (
						el.matches && el.matches( sel ) ||
						el.msMatchesSelector && el.msMatchesSelector( sel )
					);
				}

				document.body.addEventListener( 'click', function ( event ) {
					if ( ! event.target ) {
						return;
					}

					var el;
					if ( matches( event.target, 'a.share-facebook' ) ) {
						el = event.target;
					} else if ( event.target.parentNode && matches( event.target.parentNode, 'a.share-facebook' ) ) {
						el = event.target.parentNode;
					}

					if ( el ) {
						event.preventDefault();

						// If there's another sharing window open, close it.
						if ( typeof windowOpen !== 'undefined' ) {
							windowOpen.close();
						}
						windowOpen = window.open( el.getAttribute( 'href' ), 'wpcomfacebook', 'menubar=1,resizable=1,width=600,height=400' );
						return false;
					}
				} );
			} )();
</script>
	<iframe src='https://widgets.wp.com/likes/master.html?ver=20241229#ver=20241229&#038;n=1' scrolling='no' id='likes-master' name='likes-master' style='display:none;'></iframe>
	<div id='likes-other-gravatars' class='wpl-new-layout' role="dialog" aria-hidden="true" tabindex="-1"><div class="likes-text"><span>%d</span></div><ul class="wpl-avatars sd-like-gravatars"></ul></div>
			<script type="text/javascript">
			(function () {
				const iframe = document.getElementById( 'jetpack_remote_comment' );
								const watchReply = function() {
					// Check addComment._Jetpack_moveForm to make sure we don't monkey-patch twice.
					if ( 'undefined' !== typeof addComment && ! addComment._Jetpack_moveForm ) {
						// Cache the Core function.
						addComment._Jetpack_moveForm = addComment.moveForm;
						const commentParent = document.getElementById( 'comment_parent' );
						const cancel = document.getElementById( 'cancel-comment-reply-link' );

						function tellFrameNewParent ( commentParentValue ) {
							const url = new URL( iframe.src );
							if ( commentParentValue ) {
								url.searchParams.set( 'replytocom', commentParentValue )
							} else {
								url.searchParams.delete( 'replytocom' );
							}
							if( iframe.src !== url.href ) {
								iframe.src = url.href;
							}
						};

						cancel.addEventListener( 'click', function () {
							tellFrameNewParent( false );
						} );

						addComment.moveForm = function ( _, parentId ) {
							tellFrameNewParent( parentId );
							return addComment._Jetpack_moveForm.apply( null, arguments );
						};
					}
				}
				document.addEventListener( 'DOMContentLoaded', watchReply );
				// In WP 6.4+, the script is loaded asynchronously, so we need to wait for it to load before we monkey-patch the functions it introduces.
				document.querySelector('#comment-reply-js')?.addEventListener( 'load', watchReply );

				
				window.addEventListener( 'message', function ( event ) {
					if ( event.origin !== 'https://jetpack.wordpress.com' ) {
						return;
					}
					iframe.style.height = event.data + 'px';
				});
			})();
		</script>
		</body>
</html>

<!-- Cached by WP-Optimize - https://getwpo.com - Last modified: December 29, 2024 3:48 pm (Asia/Kolkata UTC:5.5) -->




## Installation

### 1. Create a Namespace

```Bash
kubectl create namespace logging
```
2. Add Helm Repositories
```Bash

helm repo add elastic [https://helm.elastic.co](https://helm.elastic.co)
helm repo update
helm repo add fluent [https://fluent.github.io/helm-charts](https://fluent.github.io/helm-charts)
helm repo update
```
3. Install Elasticsearch
```Bash

helm install elasticsearch \
  --set replicas=1 \
  --set volumeClaimTemplate.storageClassName=gp2 \
  --set persistence.labels.enabled=true \
  elastic/elasticsearch -n logging
```
4. Install Kibana
   
```Bash

helm install kibana \
  --set service.type=LoadBalancer \
  elastic/kibana -n logging
```
5. Retrieve Elasticsearch Credentials
```Bash

# for username
kubectl get secrets --namespace=logging elasticsearch-master-credentials -ojsonpath='{.data.username}' | base64 -d

# for password
kubectl get secrets --namespace=logging elasticsearch-master-credentials -ojsonpath='{.data.password}' | base64 -d
```
Important: Store these credentials securely.

6. Configure Fluent Bit
   
```
Create a fluentbit-values.yaml file.
Locate the HTTP_Passwd field and replace the placeholder with the Elasticsearch password from the previous step.
```

```YAML

service:
  type: ClusterIP
  annotations: {}
env:
  # Place your Elasticsearch credentials here
  HTTP_User: "elastic"
  HTTP_Passwd: "your_elasticsearch_password" 
```
7. Install Fluent Bit

```

helm install fluent-bit \
  fluent/fluent-bit \
  -f fluentbit-values.yaml \
  -n logging
```
Accessing Kibana
1. Get the external IP of the Kibana LoadBalancer:
```
```Bash

kubectl get service kibana -n logging
```
2. Open your browser and navigate to http://<EXTERNAL-IP>:5601.

3. Log in with the Elasticsearch credentials.

Clean Up

```Bash

helm uninstall fluent-bit -n logging
helm uninstall elasticsearch -n logging
helm uninstall kibana -n logging

kubectl delete namespace logging
```
Important Notes
* This is a basic setup. For production, consider:
  * Increased Elasticsearch replicas for high availability.
  * Resource limits for pods.
  * TLS encryption for secure communication.
  * Advanced log processing with Fluentd or Logstash.
* Refer to the official documentation for Elasticsearch, Kibana, and Fluent Bit for the latest information and advanced configuration options.
