<script setup>
import { Icon } from "@iconify/vue";
import { Camera, CameraResultType } from "@capacitor/camera";
import { useStore } from "~/store/store";
import { storeToRefs } from "pinia";

// useStore() and name handling:
const store = useStore();
const name = storeToRefs(store).name;

const newName = ref("");
function setName() {
  store.name = newName.value;
  newName.value = "";
}

//geolocation via VueUse:
const { coords, locatedAt, error, resume, pause } = useGeolocation();

// taking a picture and displaying it:
const imageUrl = ref(null);
async function takePic() {
  const image = await Camera.getPhoto({
    quality: 90,
    allowEditing: true,
    resultType: CameraResultType.Uri,
  });
  imageUrl.value = image.webPath;
}

definePageMeta({
  alias: "/home",
});
</script>

<template>
  <v-container flex flex-col items-center gap-4 prose>
    <div class="prose text-justify hyphens-auto" flex flex-col items-center>
      <h1 text-center>Hello {{ name || "Friend" }}!</h1>
      <p>
        Dieses Template soll dir helfen, einen schnellen Start ins
        Frontend/UI-Development hinzulegen. Das Herzstück der Vorlage bildet das
        Meta-Framework
        <a
          href="https://nuxt.com/docs/guide/concepts/auto-imports"
          target="_blank"
          >Nuxt</a
        >. Wir nennen Nuxt ein Meta-Framework da es auf
        <a href="https://vuejs.org/tutorial/#step-1" target="_blank">Vue</a>
        aufbaut. Es gibt drei unterschiedliche Arten von Vue-Komponenten in
        Nuxt:
      </p>
      <ol>
        <li>
          <h2>Layouts</h2>
          <p>
            Hier finden wir html-Elemente/Komponenten, die auf (fast) allen
            Seiten unserer Web-Applikation sichtbar sein sollen (z.B.
            Navigation, Header und Footer). Bei Bedarf können für einzelne
            Seiten gesonderte Layouts erstellt werden. Der Inhalt von Pages wird
            an der Stelle im Layout eingefügt, an der wir den
            <code>&lt;slot&gt;</code> tag einfügen.
          </p>
        </li>
        <li>
          <h2>Pages</h2>
          <p>
            Pages sind das Herzstück unserer Navigation. Nuxt erstellt für jeden
            Vue-Komponenten im Ordner <code>pages</code> eine eigene Route. Mit
            Hilfe von NuxtLinks kann einfach zwischen den Seiten navigiert
            werden. Beispielsweise führt uns
            <code class="language-html">&lt;NuxtLink to="/about"&gt;</code> bei
            einem Klick auf die About-Seite. Alternativ kann die navigation auch
            mit Javascript mit der <code>push()</code> Funktion des
            <code>router</code> Objekts erfolgen. Ein Beispiel dafür findet sich
            im Header-Komponenten.
          </p>
        </li>
        <li>
          <h2>Components</h2>
          <p>
            Im <code>components</code> Ordner schließlich finden wir die
            "normalen" Vue-Komponenten, d.h. Elemente unseres UIs, die wir immer
            wieder (z.B. auf unterschiedlichen Pages) verwenden können. Achtung:
            Die Ordnerstruktur spielt bei der Einbindung der Komponenten eine
            Rolle! Z.B. verwenden wir den Header-Komponenten aus dem Unterordner
            UI mit dem tag <code>&lt;UiHeader&gt;</code> oder
            <code>&lt;ui-header&gt;</code>.
          </p>
        </li>
      </ol>
      <h2>Frontend-Speicher</h2>
      <p>
        Im Ordner <code>store</code> des Projekts findest du eine js-Datei mit
        einem Pinia-Store. Durch ein installiertes Plugin werden alle
        Variablen/Konstanten aus dem Store mit dem localStorage des Browsers
        synchronisiert. Das heißt, die im Store hinterlegten Daten bleiben auch
        nach einem Reload der Seite erhalten und überstehen in den meisten
        Fällen - abhängig von den Einstellungen der Benutzer*innen - sogar einen
        Browser-Neustart. Etwas weiter unten findest du ein Textfeld zur Eingabe
        deines Namens. Sobal du deinen Namen eingegeben hast, kanns du die Seite
        auch neu laden und wirst oben anstatt "Friend" deinen Namen sehen.
      </p>
      <h2>Styling</h2>
      <p>
        Für grundlegendes Styling verwenden wir das CSS-Framework
        <em>Tailwind</em>. Einen guten Überblick über die vielen Utility-Klassen
        von Tailwind bietet nach Start des DevServers die Übersicht unter
        <a href="http://localhost:3000/_tailwind/" target="_blank"
          >http://localhost:3000/_tailwind/</a
        >. Durch die zusätzliche Integration des Frameworks
        <a href="https://uno.antfu.me/" target="_blank">UnoCSS</a> können wir
        außerdem (fast) alle Tailwind-Klassen im "Attriubtify-Mode" verwenden.
        Das heißt, wir können z.B. anstatt
        <code class="language-html">&lt;div class="flex"&gt;</code> einfach nur
        <code class="language-html">&lt;div flex&gt;</code> schreiben. Bei den
        vielen Klassen, die wir mit Tailwind setzen müssen, resultiert das
        schnell in verbesserter Übersichtlichkeit.
      </p>
      <p>
        Icons lassen sich komfortabel mit
        <a href="https://icon-sets.iconify.design/" target="_blank">Iconify</a>
        einbinden: einfaches Copy-Paste genügt:
        <code>&lt;Icon icon="fa6-solid:thumbs-up" /&gt;</code> resultiert z.B.
        in: <Icon icon="fa6-solid:thumbs-up" text-6 />
      </p>
      <p>
        Tailwind ist wunderbar, um z.B. mit <code>flex</code> oder
        <code>grid</code> Layouts zu gestalten. Bibliotheken mit fertigen
        UI-Elementen können unser Leben als Webdesigner aber wesentlich
        vereinfachen. Dieses Template integriert zwei unterschiedliche
        UI-Komponenten-Bibliotheken: <em>daisyUI</em> und <em>Vuetify</em>.
        daisyUI besteht nur aus CSS-Klassen, die auf Tailwind aufbauen und damit
        100%ig anpassbar sind. Vuetify hingegen ist eine speziell für Vue
        geschriebene Komponenten-Bibliothek. Sie enthält damit auch Javascript
        und ist in der Anwendung etwas komplexer. Im Folgenden finden sich
        Beispiele für den Einsatz beider Bibliotheken.
      </p>
    </div>
    <v-container grid md:grid-cols-2 gap-4 class="prose">
      <v-container flex flex-col items-center gap-4>
        <h3>
          <a href="https://daisyui.com/components/" class="link">daisyUI</a>
        </h3>
        <div flex gap-2>
          <input
            type="text"
            placeholder="Tell me your name"
            v-model="newName"
            class="input w-full max-w-xs bg-base-200"
            @keyup.enter="setName"
          />
          <button class="btn" @click="setName">Set Name</button>
        </div>
        <div flex flex-wrap justify-center gap-4>
          <button class="btn">Button</button>
          <button class="btn btn-primary">Primary</button>
          <button class="btn btn-secondary">Secondary</button>
          <button class="btn btn-accent">Accent</button>
          <button class="btn btn-warning btn-outline" b-1>Warning</button>
          <button class="btn btn-outline btn-error" b-1>Error</button>
          <button class="btn btn-success btn-outline" b-1>Success</button>
          <button class="btn btn-info btn-outline" b-1>Info</button>
        </div>
        <div class="card w-96 bg-base-100 shadow-xl">
          <div class="card-body">
            <div class="card-title">Card title!</div>
            <div>If a dog chews shoes whose shoes does he choose?</div>
            <div class="card-actions justify-end">
              <button class="btn btn-primary">Buy Now</button>
            </div>
          </div>
        </div>
      </v-container>
      <v-container flex flex-col items-center gap-4>
        <h3>
          <a href="https://next.vuetifyjs.com/en/components/all/" class="link"
            >Vuetify</a
          >
        </h3>

        <div flex gap-2 w="400px" max-w="100%" items-center>
          <v-text-field
            label="Tell me your name"
            v-model="newName"
            @keyup.enter="setName"
          ></v-text-field>
          <v-btn @click="setName">Set Name</v-btn>
        </div>
        <div flex flex-wrap justify-center gap-4>
          <v-btn> Button </v-btn>
          <v-btn color="primary">Primary</v-btn>
          <v-btn color="secondary">Secondary</v-btn>
          <v-btn color="accent">Accent</v-btn>
          <v-btn color="warning" variant="outlined">Warning</v-btn>
          <v-btn color="error" variant="outlined">Error</v-btn>
          <v-btn color="success" variant="outlined">Success</v-btn>
          <v-btn color="info" variant="outlined">Info</v-btn>
        </div>

        <v-card>
          <v-card-title>v-card-title</v-card-title>
          <v-card-subtitle>v-card-subtitle</v-card-subtitle>
          <v-card-text>
            <span>&lt;v-card-text </span>
            <span text-red>text-red </span>
            <span text-blue>text-blue </span>
            <span text-orange>text-orange</span>&gt;
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary">Primary-Button</v-btn>
            <v-btn color="secondary">Secondary-Button</v-btn>
          </v-card-actions>
        </v-card>
      </v-container>
    </v-container>
    <div class="prose text-justify hyphens-auto" flex flex-col items-center>
      <h2>App-Funktionen</h2>
      <p>
        Durch ein Plugin wird diese Website zur
        <em>Progressive Web App</em> (PWA). Das heißt, sie ist über den Chrome
        Browser "installierbar" und verfügt zumindest über eingeschränkte
        Offline-Funktionalität. Diverse App-Logos und das Favicon der Seite
        werden aus der Datei <code>/public/icon.png</code> automatisch
        generiert. Der Titel der Website und der Name bei der Installation
        stammt aus der <code>package.json</code>.
      </p>

      <p>
        Mit Hilfe des Plugins
        <a href="https://vueuse.org/guide/" target="_blank">VueUse</a> lassen
        sich viele nützliche Funktionen relativ einfach nutzen. Zum Beispiel
        kann damit der Standort einer Person abgerufen werden:
      </p>

      <ul>
        <li>Breitengrad: {{ coords.latitude }}</li>
        <li>Längengrad: {{ coords.longitude }}</li>
      </ul>

      <p>
        Zusätzlich ist <em>Capacitor</em> installiert. Dabei handelt es sich um
        eine Programmbibliothek, die deine Web-Applikation in eine native
        Android oder iOS App "verpackt" und den Zugriff auf diverse
        Geräte-Funktionen, wie die Kamera, das Dateisystem oder Push-Nachrichten
        erlaubt. Der Kamera-Button demonstriert den Einsatz der Kamera-Funktion:
      </p>
      <v-btn @click="takePic"
        ><Icon text-6 icon="material-symbols:photo-camera-rounded"
      /></v-btn>
      <img :src="imageUrl" max-w="100%" />
    </div>
  </v-container>
</template>

<style>
/* * {
  border: 1px solid red;
} */
</style>
