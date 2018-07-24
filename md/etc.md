	function isAndroid() {
	const ua = navigator.userAgent.toLowerCase();
        return /android/.test(ua);
    }
	function go(href) {
    if (!isAndroid()) {
        href = href.replace('http:', 'intent:');
        intent = '#Intent;scheme=http;action=android.intent.action.VIEW;package=com.android.chrome;end';
    }
	location.href = href;
	}
