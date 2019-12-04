<template>
	<Page>
		<ActionBar title="Welcome to NativeScript-Vue!" />
		<GridLayout columns="*" rows="*">
			<webview :src="processHTML('hello world')"></webview>
		</GridLayout>
	</Page>
</template>

<script>
    const fileSystemModule = require("tns-core-modules/file-system");

	export default {
		data() {
			return {
				msg: 'Hello World!'
			}
		},

        methods: {
            currentApp(path) {
                return encodeURI("file:///" + fileSystemModule.knownFolders.currentApp().path + "/" + path);
            },

	        readSync(path) {
		        let file = fileSystemModule.knownFolders.currentApp().getFile(path);
		        return file.readTextSync();
	        },

            processHTML(html) {
                return `
					<html>
						<head>
							<meta charset="UTF-8">
							<meta http-equiv="X-UA-Compatible" content="ie=edge">
							<meta name="HandheldFriendly" content="True">

							<style>
								${this.readSync('assets/webview/basic.css')}
							</style>
						</head>
						<body class="generic-page-theme">
							<div id="updatable">
								${html}
							</div>

                            ${`<script>`}
                                // fixes issue that prevents zoom-in on iOS
                                setTimeout(function() {
                                    var meta = document.querySelector('meta[name="viewport"]');
                                    if (meta) meta.setAttribute('content', "width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=0.0, viewport-fit=auto");
                                }, 0);
                            ${'</' + 'script>'}
						</body>
					</html>
				`;
            }
        }
	}
</script>

<style scoped>
	ActionBar {
		background-color: #53ba82;
		color: #ffffff;
	}

	.message {
		vertical-align: center;
		text-align: center;
		font-size: 20;
		color: #333333;
	}
</style>
