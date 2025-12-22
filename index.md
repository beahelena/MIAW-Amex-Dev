<html>
  <body>
	<script>
	   var FIRST_NAME = "Fernanda";
	   var LAST_NAME = "Silva";
	   var EMAIL = "silva@test.com";
	   var PHONE = "98766644";

	   var CLIENT_ID = "1a2b3c4u99";
	   var COUNTRY_OF_CARD = "Brazil";
	   var BANK_NAME = "VISA";
	
	   var BIN_NUMBER = "1345699999";
	   var BIN_NUMBER_MASKED = BIN_NUMBER.slice(0, -4) + "****";

       console.log({ FIRST_NAME, LAST_NAME, EMAIL, PHONE, CLIENT_ID,
              COUNTRY_OF_CARD, BANK_NAME, BIN_NUMBER, BIN_NUMBER_MASKED }); 

		// 🆕 NOVA FUNÇÃO: Verificar se deve abrir chat automaticamente
       function shouldAutoOpenChat() {
           const urlParams = new URLSearchParams(window.location.search);
           return urlParams.get('chat') === 'true' || urlParams.get('openchat') === '1';
       }
	
		window.addEventListener("onEmbeddedMessagingReady", function (e) {

		const userAgent = navigator.userAgent;
		
		const fetchBrowserInfo = () => {
			let name = "Unknown";
			let version = "";

			if (userAgent.includes("OPR") || userAgent.includes("Opera")) {
				name = "Opera";
				version = userAgent.match(/OPR\/([\d.]+)/)?.[1] || userAgent.match(/Opera\/([\d.]+)/)?.[1];
			} else if (userAgent.includes("Edg")) {
				name = "Edge";
				version = userAgent.match(/Edg\/([\d.]+)/)?.[1];
			} else if (userAgent.includes("Chrome") && !userAgent.includes("Edg") && !userAgent.includes("OPR")) {
				name = "Chrome";
				version = userAgent.match(/Chrome\/([\d.]+)/)?.[1];
			} else if (userAgent.includes("Safari") && !userAgent.includes("Chrome")) {
				name = "Safari";
				version = userAgent.match(/Version\/([\d.]+)/)?.[1];
			} else if (userAgent.includes("Firefox")) {
				name = "Firefox";
				version = userAgent.match(/Firefox\/([\d.]+)/)?.[1];
			}

			return `${name} ${version}`;
		};

		const browserInfo = fetchBrowserInfo();
		const browserLanguage = navigator.language;
		const browserPlatform = navigator.platform;
		const screenResolution = `${window.screen.width}x${window.screen.height}`;

		embeddedservice_bootstrap.prechatAPI.setVisiblePrechatFields({
		  "_firstName": {
			"value": FIRST_NAME,
			"isEditableByEndUser": true
		  },
		  "_lastName": {
			"value": LAST_NAME,
			"isEditableByEndUser": true
		  },
		  "Bin_Number": {
			"value": BIN_NUMBER_MASKED,
			"isEditableByEndUser": true
		  }
		});
		
		embeddedservice_bootstrap.prechatAPI.setHiddenPrechatFields({
		  "Email" : " ",
		  "Phone" : " ",
		  "Client_ID" : " ",
		  "Country_Of_Card" : " ",
		  "Bank_Name" : " ",
		  "BrowserName" : browserInfo,
		  "BrowserLanguage" : browserLanguage,
		  "BrowserPlatform" : browserPlatform,
		  "UserAgent" : " ",
		  "ScreenResolution" : screenResolution,
		  "X10_first_digits_of_the_Card" : " "
				
		});

		// 🆕 NOVO: Abrir chat automaticamente se parâmetro estiver na URL
        if (shouldAutoOpenChat()) {
    console.log('Parâmetro de chat detectado na URL. Abrindo chat automaticamente...');
    setTimeout(function() {
        try {
            // Tentativas diferentes de API
            if (embeddedservice_bootstrap.utilAPI) {
                // Opção 1
                embeddedservice_bootstrap.utilAPI.maximizeWidget();
                console.log('Chat aberto com maximizeWidget!');
            } else if (embeddedservice_bootstrap.utilAPI) {
                // Opção 2
                embeddedservice_bootstrap.utilAPI.showWidget();
                console.log('Chat aberto com showWidget!');
            }
        } catch (error) {
            console.error('Erro ao abrir chat automaticamente:', error);
        }
    }, 3000); // Aumentei para 3 segundos
}
	  });
   
      function initEmbeddedMessaging() {
        try {

			//let browserLanguage = navigator.language || navigator.userLanguage || 'en-US';
			//console.log('First Information:', browserLanguage);
			//let supportedLanguages = ['en-US', 'es', 'pt-BR'];
			//let finalLanguage = supportedLanguages.includes(browserLanguage) ? browserLanguage : 'en-US';
			//console.log('Final language to use:', finalLanguage);
			//embeddedservice_bootstrap.settings.language = finalLanguage;

			//embeddedservice_bootstrap.settings.language = finalLanguage;
			//console.log('Idioma definido:', finalLanguage);	

   			//Using Variables:            
            var LANGUAGE = 'Spanish';
            var languageMap = {
            'Portuguese': 'pt-BR',
            'Spanish': 'es',
            'English': 'en-US'
            };
            var finalLanguage = languageMap[LANGUAGE] || 'en-US';
            embeddedservice_bootstrap.settings.language = finalLanguage;
            console.log('Defined language:', finalLanguage);

			// 🆕 NOVO: Configurações para abrir automaticamente
            embeddedservice_bootstrap.settings.defaultMinimized = !shouldAutoOpenChat();

			embeddedservice_bootstrap.init(
				'00DOx000002jjB7',
		        'Amex_External_Website',
		        'https://axaus-travel--dev.sandbox.my.site.com/ESWAmexExternalWebsite1756993093717',
		        {
		          scrt2URL: 'https://axaus-travel--dev.sandbox.my.salesforce-scrt.com'
		        }
			);
		} catch (err) {
			console.error('Error loading Embedded Messaging: ', err);
		}
      }
    </script>

       <script 
            type='text/javascript' 
            src='https://axaus-travel--dev.sandbox.my.site.com/ESWAmexExternalWebsite1756993093717/assets/js/bootstrap.min.js'                onload='initEmbeddedMessaging()'>

</script>

  </body>
</html>
