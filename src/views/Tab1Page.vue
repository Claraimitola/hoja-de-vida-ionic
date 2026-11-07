<template>
  <ion-page>
    <ion-content :fullscreen="true" class="fondo">
      <!-- ===== ENCABEZADO: NOMBRE E IDENTIDAD ===== -->
      <section class="hero">
        <div class="avatar">{{ iniciales }}</div>
        <h1>{{ persona.nombre }} {{ persona.apellido }}</h1>
        <p class="profesion">{{ persona.profesion }}</p>
        <span class="badge">
          <ion-icon :icon="idCardOutline"></ion-icon>
          C.C. {{ persona.documento }}
        </span>
      </section>

      <div class="contenedor">
        <!-- ===== SEPARADOR ===== -->
        <div class="separador"><span></span></div>

        <!-- ===== DATOS PERSONALES ===== -->
        <section class="tarjeta">
          <h2 class="titulo-seccion">
            <ion-icon :icon="personOutline"></ion-icon> Datos personales
          </h2>
          <div class="grid-datos">
            <div class="dato" v-for="dato in datosPersonales" :key="dato.etiqueta">
              <div class="icono-dato"><ion-icon :icon="dato.icono"></ion-icon></div>
              <div>
                <small>{{ dato.etiqueta }}</small>
                <p>{{ dato.valor }}</p>
              </div>
            </div>
          </div>
        </section>

        <!-- ===== EXPERIENCIA ACADÉMICA ===== -->
        <section class="tarjeta">
          <h2 class="titulo-seccion">
            <ion-icon :icon="schoolOutline"></ion-icon> Experiencia académica
          </h2>
          <div class="timeline">
            <div class="evento" v-for="estudio in academica" :key="estudio.titulo">
              <span class="punto"></span>
              <span class="periodo">{{ estudio.periodo }}</span>
              <h3>{{ estudio.titulo }}</h3>
              <p>{{ estudio.institucion }}</p>
            </div>
          </div>
        </section>

        <!-- ===== EXPERIENCIA LABORAL ===== -->
        <section class="tarjeta">
          <h2 class="titulo-seccion">
            <ion-icon :icon="briefcaseOutline"></ion-icon> Experiencia laboral
          </h2>
          <div class="timeline">
            <div class="evento" v-for="trabajo in laboral" :key="trabajo.cargo + trabajo.empresa">
              <span class="punto"></span>
              <span class="periodo">{{ trabajo.periodo }}</span>
              <h3>{{ trabajo.cargo }}</h3>
              <p class="empresa">{{ trabajo.empresa }}</p>
              <p>{{ trabajo.descripcion }}</p>
            </div>
          </div>
        </section>

        <!-- ===== ME GUSTA / NO ME GUSTA ===== -->
        <section class="tarjeta votos">
          <h2>¿Te gustó mi perfil?</h2>
          <div class="botones-voto">
            <button class="voto like" :class="{ pop: animarLike }" @click="votar('like')">
              <ion-icon :icon="thumbsUp"></ion-icon>
              <span>{{ meGusta }}</span>
            </button>
            <button class="voto dislike" :class="{ pop: animarDislike }" @click="votar('dislike')">
              <ion-icon :icon="thumbsDown"></ion-icon>
              <span>{{ noMeGusta }}</span>
            </button>
          </div>
          <div class="barra-votos" v-if="totalVotos > 0">
            <div class="relleno" :style="{ width: porcentajeLike + '%' }"></div>
          </div>
          <small v-if="totalVotos > 0">{{ porcentajeLike }}% de aprobación</small>
          <small v-else>Sé el primero en votar</small>
        </section>
      </div>
    </ion-content>

    <!-- ===== BARRA DE ESTADO ===== -->
    <ion-footer class="ion-no-border">
      <div class="barra-estado">
        <div class="estado">
          <span class="punto-verde"></span>
          <div>
            <strong>Disponible</strong>
            <small>{{ totalVotos }} votos</small>
          </div>
        </div>
        <a class="boton-sitio" :href="miSitio" target="_blank">
          <ion-icon :icon="globeOutline"></ion-icon>
          Mi sitio
        </a>
      </div>
    </ion-footer>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, reactive, computed } from 'vue';
import { IonPage, IonContent, IonFooter, IonIcon } from '@ionic/vue';
import {
  personOutline, idCardOutline, locationOutline, mailOutline, callOutline,
  maleFemaleOutline, calendarOutline, schoolOutline, briefcaseOutline,
  thumbsUp, thumbsDown, globeOutline,
} from 'ionicons/icons';

// ---- Cambia estos datos por los tuyos ----
const persona = reactive({
  nombre: 'Clara Isabel',
  apellido: 'Imitola Teran',
  documento: '1.234.567.890',
  profesion: 'Estudiante de Ingeniería de Software',
  direccion: 'Cartagena, Bolívar',
  correo: 'clara.imitola@unicolombo.edu.co',
  telefono: '300 000 0000',
  genero: 'Femenino',
  fechaNacimiento: '20/05/2004',
});

const academica = ref([
  { titulo: 'Ingeniería de Software (en curso)', institucion: 'Universidad', periodo: '2023 - Actualidad' },
  { titulo: 'Curso de Desarrollo Web', institucion: 'Plataforma en línea', periodo: '2024' },
  { titulo: 'Bachiller académico', institucion: 'Colegio', periodo: '2022' },
]);

const laboral = ref([
  { cargo: 'Desarrolladora web freelance', empresa: 'Independiente', periodo: '2025 - Actualidad', descripcion: 'Páginas web para pequeños negocios.' },
  { cargo: 'Auxiliar de sistemas', empresa: 'Empresa X', periodo: '2024', descripcion: 'Soporte técnico y mantenimiento de equipos.' },
]);

const miSitio = 'https://github.com/';
// -------------------------------------------

const iniciales = computed(
  () => (persona.nombre.charAt(0) + persona.apellido.charAt(0)).toUpperCase()
);

const datosPersonales = computed(() => [
  { etiqueta: 'Nombre', valor: persona.nombre, icono: personOutline },
  { etiqueta: 'Apellido', valor: persona.apellido, icono: personOutline },
  { etiqueta: 'Documento', valor: persona.documento, icono: idCardOutline },
  { etiqueta: 'Dirección', valor: persona.direccion, icono: locationOutline },
  { etiqueta: 'Correo', valor: persona.correo, icono: mailOutline },
  { etiqueta: 'Teléfono', valor: persona.telefono, icono: callOutline },
  { etiqueta: 'Género', valor: persona.genero, icono: maleFemaleOutline },
  { etiqueta: 'Nacimiento', valor: persona.fechaNacimiento, icono: calendarOutline },
]);

// Votos (reactividad)
const meGusta = ref(0);
const noMeGusta = ref(0);
const animarLike = ref(false);
const animarDislike = ref(false);

const totalVotos = computed(() => meGusta.value + noMeGusta.value);
const porcentajeLike = computed(() =>
  totalVotos.value === 0 ? 0 : Math.round((meGusta.value / totalVotos.value) * 100)
);

function votar(tipo: 'like' | 'dislike') {
  if (tipo === 'like') {
    meGusta.value++;
    animarLike.value = true;
    setTimeout(() => (animarLike.value = false), 300);
  } else {
    noMeGusta.value++;
    animarDislike.value = true;
    setTimeout(() => (animarDislike.value = false), 300);
  }
}
</script>

<style scoped>
/* ---------- Colores (modo claro) ---------- */
.fondo, .barra-estado {
  --principal: #6c5ce7;
  --secundario: #00b4d8;
  --texto: #1e1e2f;
  --suave: #6b7280;
  --tarjeta: #ffffff;
  --fondo: #f4f5fb;
  --borde: #e5e7eb;
}
/* ---------- Colores (modo oscuro) ---------- */
@media (prefers-color-scheme: dark) {
  .fondo, .barra-estado {
    --texto: #f1f1f6;
    --suave: #a0a3b1;
    --tarjeta: #1f2030;
    --fondo: #13141f;
    --borde: #2e3044;
  }
}

.fondo { --background: var(--fondo); color: var(--texto); }

/* ---------- Encabezado ---------- */
.hero {
  background: linear-gradient(135deg, var(--principal), var(--secundario));
  color: #fff;
  text-align: center;
  padding: calc(env(safe-area-inset-top) + 40px) 20px 70px;
  border-radius: 0 0 32px 32px;
}
.avatar {
  width: 96px; height: 96px;
  margin: 0 auto 12px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  border: 3px solid #fff;
  display: grid; place-items: center;
  font-size: 34px; font-weight: 700;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
}
.hero h1 { margin: 0; font-size: 26px; font-weight: 700; }
.profesion { margin: 6px 0 14px; opacity: 0.9; }
.badge {
  display: inline-flex; align-items: center; gap: 6px;
  background: rgba(255, 255, 255, 0.2);
  padding: 6px 14px; border-radius: 20px; font-size: 14px;
}

/* ---------- Contenedor ---------- */
.contenedor {
  max-width: 720px;
  margin: -40px auto 0;
  padding: 0 16px 24px;
}

/* ---------- Separador ---------- */
.separador { display: flex; justify-content: center; margin-bottom: 20px; }
.separador span {
  width: 60%; height: 4px; border-radius: 4px;
  background: linear-gradient(90deg, transparent, var(--principal), var(--secundario), transparent);
  margin-top: 52px;
}

/* ---------- Tarjetas ---------- */
.tarjeta {
  background: var(--tarjeta);
  border-radius: 20px;
  padding: 20px;
  margin-bottom: 18px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.06);
  border: 1px solid var(--borde);
}
.titulo-seccion {
  display: flex; align-items: center; gap: 8px;
  margin: 0 0 16px; font-size: 18px; color: var(--principal);
}

/* ---------- Datos personales ---------- */
.grid-datos {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 12px;
}
.dato { display: flex; align-items: center; gap: 12px; }
.icono-dato {
  width: 40px; height: 40px; flex-shrink: 0;
  border-radius: 12px;
  background: rgba(108, 92, 231, 0.12);
  color: var(--principal);
  display: grid; place-items: center; font-size: 20px;
}
.dato small { color: var(--suave); font-size: 12px; }
.dato p { margin: 2px 0 0; font-weight: 600; word-break: break-word; }

/* ---------- Línea de tiempo ---------- */
.timeline { position: relative; padding-left: 22px; }
.timeline::before {
  content: ''; position: absolute; left: 6px; top: 4px; bottom: 4px;
  width: 2px; background: var(--borde);
}
.evento { position: relative; padding-bottom: 18px; }
.evento:last-child { padding-bottom: 0; }
.punto {
  position: absolute; left: -22px; top: 4px;
  width: 14px; height: 14px; border-radius: 50%;
  background: linear-gradient(135deg, var(--principal), var(--secundario));
  box-shadow: 0 0 0 4px var(--tarjeta);
}
.periodo {
  font-size: 12px; font-weight: 600; color: var(--secundario);
  text-transform: uppercase; letter-spacing: 0.5px;
}
.evento h3 { margin: 4px 0 2px; font-size: 16px; }
.evento p { margin: 2px 0; color: var(--suave); font-size: 14px; }
.evento .empresa { color: var(--principal); font-weight: 500; }

/* ---------- Votos ---------- */
.votos { text-align: center; }
.votos h2 { margin: 0 0 16px; font-size: 18px; }
.botones-voto { display: flex; justify-content: center; gap: 16px; margin-bottom: 16px; }
.voto {
  display: flex; align-items: center; gap: 8px;
  padding: 12px 24px; border-radius: 30px;
  border: none; cursor: pointer;
  font-size: 18px; font-weight: 700;
  transition: transform 0.15s ease;
}
.voto ion-icon { font-size: 22px; }
.voto:active { transform: scale(0.92); }
.like { background: rgba(16, 185, 129, 0.15); color: #10b981; }
.dislike { background: rgba(239, 68, 68, 0.15); color: #ef4444; }
.pop { animation: pop 0.3s ease; }
@keyframes pop {
  50% { transform: scale(1.2); }
}
.barra-votos {
  height: 8px; border-radius: 8px; overflow: hidden;
  background: rgba(239, 68, 68, 0.3);
  margin-bottom: 8px;
}
.relleno { height: 100%; background: #10b981; transition: width 0.3s ease; }
.votos small { color: var(--suave); }

/* ---------- Barra de estado ---------- */
.barra-estado {
  display: flex; align-items: center; justify-content: space-between;
  padding: 10px 16px;
  background: linear-gradient(135deg, var(--principal), var(--secundario));
  color: #fff;
}
.estado { display: flex; align-items: center; gap: 10px; }
.estado div { display: flex; flex-direction: column; line-height: 1.2; }
.estado small { opacity: 0.85; font-size: 12px; }
.punto-verde {
  width: 10px; height: 10px; border-radius: 50%;
  background: #4ade80;
  box-shadow: 0 0 0 4px rgba(74, 222, 128, 0.3);
}
.boton-sitio {
  display: flex; align-items: center; gap: 6px;
  background: #fff; color: var(--principal);
  padding: 8px 16px; border-radius: 20px;
  font-weight: 600; text-decoration: none;
}
</style>