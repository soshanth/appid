---

copyright:
  years: 2017
lastupdated: "2017-11-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock: .codeblock}

# Configuration du logiciel SDK Android
{: #android-sdk}

Construisez vos applications Android avec le SDK client d'{{site.data.keyword.appid_short}}, initialisez le SDK, authentifiez les utilisateurs et soumettez des demandes à des ressources protégées et non protégées.
{:shortdesc}


## Avant de commencer
{: #before-you-begin}

Vous devez disposer des éléments suivants :
  * Une instance du service {{site.data.keyword.appid_short_notm}}.
  * Votre ID titulaire.
    * Dans l'onglet **Données d'identification pour le
service** de votre tableau de bord du service, cliquez sur **Afficher les données d'identification**. Votre ID titulaire est affiché dans la zone **tenantID**. Cette valeur est utilisée pour initialiser votre application.
  * Votre région {{site.data.keyword.Bluemix}}. Vous pouvez identifier votre région en recherchant dans l'interface utilisateur. Cette valeur est utilisée pour initialiser votre application.
    <table> <caption> Tableau 1. Régions {{site.data.keyword.Bluemix_notm}} et valeurs de SDK correspondantes </caption>
    <tr>
      <th> Région Bluemix </th>
      <th> Valeur du SDK </th>
    </tr>
    <tr>
      <td> Sud des Etats-Unis </td>
      <td> AppID.REGION_US_SOUTH </td>
    </tr>
    <tr>
      <td> Sydney </td>
      <td> AppID.REGION_SYDNEY </td>
    </tr>
    <tr>
      <td> Royaume-Uni </td>
      <td> AppID.REGION_UK </td>
    </tr>
  </table>

  * Un projet <a href="https://developers.google.com/web/tools/setup/" target="_blank">Android Studio<img src="../../icons/launch-glyph.svg" alt="icône de lien externe"></a>, configuré pour
fonctionner avec Gradle.

## Installation du SDK client
{: #install-appid-sdk}

1. Créez un projet Android Studio ou ouvrez un projet existant.
2.  Ajoutez le référentiel JitPack à votre fichier racine `build.gradle`.

  ```gradle
    allprojects {
	    repositories {
		    ...
		    maven { url 'https://jitpack.io' }
	    }
    }
  ```
  {: codeblock}

3. Ouvrez le fichier `build.gradle` de votre application.

    **Remarque** : assurez-vous d'ouvrir le fichier
correspondant à
votre application et non pas le fichier `build.gradle` du projet.
4. Recherchez la section dependencies dans le fichier et
ajoutez une dépendance de compilation pour le SDK client
d'{{site.data.keyword.appid_short_notm}} .

  ```gradle
   dependencies {
       compile group: 'com.github.ibm-cloud-security:appid-clientsdk-android:1.+'
   }
  ```
  {: codeblock}

5. Recherchez la section defaultConfig et ajoutez les lignes de code ci-dessous.

  ```gradle
  defaultConfig {
  ...
  manifestPlaceholders = ['appIdRedirectScheme': android.defaultConfig.applicationId]
  }
  ```
  {: codeblock}

6. Synchronisez votre projet avec Gradle. Cliquez sur **Tools** > **Android** > **Sync Project with Gradle Files (Synchroniser le projet avec les fichiers Gradle)**.

## Initialisation du SDK client
{: #initialize-client-sdk}

Initialisez le SDK client en transmettant les paramètres de contexte, d'ID titulaire et de région à la méthode initialize. Bien que ce ne soit pas obligatoire, le code d'initialisation est souvent placé dans la méthode onCreate de l'activité principale dans votre application Android.

  ```java
  AppID.getInstance().initialize(getApplicationContext(), <tenantId>, AppID.REGION_UK);
  ```
  {: codeblock}

1. Remplacez *tenantId* par l'ID titulaire du service
{{site.data.keyword.appid_short_notm}}.
2. Remplacez AppID.REGION_UK par votre région {{site.data.keyword.Bluemix_notm}}.


## Authentification des utilisateurs à l'aide du widget de connexion
{: #authenticate-login-widget}

La configuration par défaut du widget de connexion utilise Facebook et Google comme options d'authentification. Si vous ne configurez qu'une seule option, le widget de connexion ne démarre pas
et
l'utilisateur est redirigé vers l'écran d'authentification du fournisseur d'identité
(IDP) configuré.



Une fois que le SDK client d'{{site.data.keyword.appid_short_notm}} est initialisé, vous pouvez authentifier vos utilisateurs en exécutant le widget de connexion.

  ```java
  LoginWidget loginWidget = AppID.getInstance().getLoginWidget();
  loginWidget.launch(this, new AuthorizationListener() {
        @Override
        public void onAuthorizationFailure (AuthorizationException exception) {
          //Exception occurred
        }

        @Override
        public void onAuthorizationCanceled () {
          //Authentication canceled by the user
        }

        @Override
        public void onAuthorizationSuccess (AccessToken accessToken, IdentityToken identityToken) {
          //User authenticated
        }
      });
  ```
  {: codeblock}


## Accès aux attributs utilisateur
{: #accessing}

En obtenant un jeton d'accès, vous pouvez accéder au noeud final des attributs utilisateur protégés. Vous
pouvez y accéder avec les méthodes ci-dessous.

  ```java
  void setAttribute(@NonNull String name, @NonNull String value, UserAttributeResponseListener listener);
  void setAttribute(@NonNull String name, @NonNull String value, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

  void getAttribute(@NonNull String name, UserAttributeResponseListener listener);
  void getAttribute(@NonNull String name, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

  void deleteAttribute(@NonNull String name, UserAttributeResponseListener listener);
  void deleteAttribute(@NonNull String name, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

  void getAllAttributes(@NonNull UserAttributeResponseListener listener);
  void getAllAttributes(@NonNull AccessToken accessToken, @NonNull UserAttributeResponseListener listener);
  ```
  {: codeblock}

Lorsqu'un jeton d'accès n'est pas transmis explicitement, {{site.data.keyword.appid_short_notm}} utilise le dernier jeton reçu.

Par exemple, vous pouvez utiliser le code ci-dessous pour définir un
nouvel
attribut ou remplacer un attribut existant.

  ```java
  appId.getUserAttributeManager().setAttribute(name, value, useThisToken,new UserAttributeResponseListener() {
		@Override
		public void onSuccess(JSONObject attributes) {
			//attributes received in JSON format on successful response 		}

		@Override 		public
void onFailure(UserAttributesException e) {
			//Exception occurred 		}
	});
  ```
  {: codeblock}

### Connexion anonyme
{: #anonymous notoc}

Avec {{site.data.keyword.appid_short_notm}}, vous pouvez vous connecter de
manière [anonyme](/docs/services/appid/user-profile.html#anonymous).

  ```java
  appId.loginAnonymously(getApplicationContext(), new AuthorizationListener() {
		@Override
		public void onAuthorizationFailure(AuthorizationException exception) {
			//Exception occurred 		}

		@Override
		public void onAuthorizationCanceled() {
			//Authentication canceled by the user 		}

		@Override
		public void onAuthorizationSuccess(AccessToken accessToken, IdentityToken identityToken) {
			//User authenticated 		}
	});
  ```
  {: codeblock}

### Authentification progressive
{: #progressive notoc}

Lorsqu'il dispose d'un jeton d'accès anonyme, l'utilisateur peut devenir un utilisateur identifié en transmettant ce jeton à la méthode `loginWidget.launch`.

  ```java
  void launch (@NonNull final Activity activity, @NonNull final AuthorizationListener authorizationListener, String accessTokenString);
  ```
  {: codeblock}

Après une connexion anonyme, une authentification progressive a lieu même si le widget de connexion est appelé sans transmission d'un jeton d'accès vu que le service a utilisé le dernier jeton d'accès reçu. Si
vous voulez effacer les jetons que vous avez stockés, exécutez la commande ci-dessous.

  ```java
  	appIDAuthorizationManager = new AppIDAuthorizationManager(this.appId);
  appIDAuthorizationManager.clearAuthorizationData();
  ```
  {: codeblock}


## Etapes suivantes
{: #next-steps}

{{site.data.keyword.appid_short_notm}} fournit une configuration par défaut lorsque vous mettez en place initialement vos fournisseurs d'identité. Vous ne pouvez utiliser la configuration par défaut qu'en mode développement. Avant de publier votre application, [mettez à jour la configuration par défaut avec vos propres données d'identification](/docs/services/appid/identity-providers.html).
