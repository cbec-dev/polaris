<script setup>
import { ref } from 'vue'
import PlatformLayout from '../../PlatformLayout.vue'
import Checkbox from "../../Checkbox.vue";

const props = defineProps([
  'platform',
  'config'
])

const config = ref(props.config)
</script>

<template>
  <div class="config-page">
    <section class="settings-section">
      <div class="settings-section-header">
        <div class="section-kicker">Load handling</div>
        <h3 class="settings-section-title">Stream behavior under load</h3>
      </div>

      <div class="mb-3">
        <label for="fec_percentage" class="block text-sm font-medium text-storm mb-1">{{ $t('config.fec_percentage') }}</label>
        <input type="text" class="w-full bg-deep border border-storm rounded-lg px-3 py-2 text-silver focus:border-ice focus:outline-none" id="fec_percentage" placeholder="20" v-model="config.fec_percentage" />
        <div class="text-sm text-storm mt-1">{{ $t('config.fec_percentage_desc') }}</div>
      </div>

      <div class="mb-3">
        <label for="qp" class="block text-sm font-medium text-storm mb-1">{{ $t('config.qp') }}</label>
        <input type="number" class="w-full bg-deep border border-storm rounded-lg px-3 py-2 text-silver focus:border-ice focus:outline-none" id="qp" placeholder="28" v-model="config.qp" />
        <div class="text-sm text-storm mt-1">{{ $t('config.qp_desc') }}</div>
      </div>

      <div class="mb-3">
        <label for="min_threads" class="block text-sm font-medium text-storm mb-1">{{ $t('config.min_threads') }}</label>
        <input type="number" class="w-full bg-deep border border-storm rounded-lg px-3 py-2 text-silver focus:border-ice focus:outline-none" id="min_threads" placeholder="2" min="1" v-model="config.min_threads" />
        <div class="text-sm text-storm mt-1">{{ $t('config.min_threads_desc') }}</div>
      </div>

      <Checkbox class="mb-3"
                id="limit_framerate"
                locale-prefix="config"
                v-model="config.limit_framerate"
                default="true"
      ></Checkbox>
    </section>

    <details class="settings-section settings-section-compact settings-disclosure">
      <summary class="settings-disclosure-summary">
        <div>
          <div class="section-kicker">Compatibility</div>
          <h3 class="settings-section-title mt-2">Client and environment behavior</h3>
          <div class="settings-summary-copy">Use these flags only when you need legacy behavior or broader codec advertising.</div>
        </div>
        <svg class="settings-disclosure-chevron h-4 w-4 text-storm" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 9-7 7-7-7" /></svg>
      </summary>

      <div class="settings-disclosure-body">

      <Checkbox class="mb-3"
                id="envvar_compatibility_mode"
                locale-prefix="config"
                v-model="config.envvar_compatibility_mode"
                default="false"
      ></Checkbox>

      <Checkbox class="mb-3"
                id="legacy_ordering"
                locale-prefix="config"
                v-model="config.legacy_ordering"
                default="false"
      ></Checkbox>

      <Checkbox class="mb-3"
                id="ignore_encoder_probe_failure"
                locale-prefix="config"
                v-model="config.ignore_encoder_probe_failure"
                default="false"
      ></Checkbox>

      <Checkbox class="mb-3"
                id="browser_streaming"
                locale-prefix="config"
                v-model="config.browser_streaming"
                default="false"
      ></Checkbox>

      <div class="mb-3">
        <label for="hevc_mode" class="block text-sm font-medium text-storm mb-1">{{ $t('config.hevc_mode') }}</label>
        <select id="hevc_mode" class="w-full bg-deep border border-storm rounded-lg px-3 py-2 text-silver focus:border-ice focus:outline-none" v-model="config.hevc_mode">
          <option value="0">{{ $t('config.hevc_mode_0') }}</option>
          <option value="1">{{ $t('config.hevc_mode_1') }}</option>
          <option value="2">{{ $t('config.hevc_mode_2') }}</option>
          <option value="3">{{ $t('config.hevc_mode_3') }}</option>
        </select>
        <div class="text-sm text-storm mt-1">{{ $t('config.hevc_mode_desc') }}</div>
      </div>

      <div class="mb-3">
        <label for="av1_mode" class="block text-sm font-medium text-storm mb-1">{{ $t('config.av1_mode') }}</label>
        <select id="av1_mode" class="w-full bg-deep border border-storm rounded-lg px-3 py-2 text-silver focus:border-ice focus:outline-none" v-model="config.av1_mode">
          <option value="0">{{ $t('config.av1_mode_0') }}</option>
          <option value="1">{{ $t('config.av1_mode_1') }}</option>
          <option value="2">{{ $t('config.av1_mode_2') }}</option>
          <option value="3">{{ $t('config.av1_mode_3') }}</option>
        </select>
        <div class="text-sm text-storm mt-1">{{ $t('config.av1_mode_desc') }}</div>
      </div>
      </div>
    </details>

    <details class="settings-section settings-section-compact settings-disclosure">
      <summary class="settings-disclosure-summary">
        <div>
          <div class="section-kicker">Override</div>
          <h3 class="settings-section-title mt-2">Capture and encoder preference</h3>
          <div class="settings-summary-copy">Force a specific capture or encoder path only when automatic selection is not working.</div>
        </div>
        <svg class="settings-disclosure-chevron h-4 w-4 text-storm" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 9-7 7-7-7" /></svg>
      </summary>

      <div class="settings-disclosure-body">

      <div class="mb-3" v-if="platform !== 'macos'">
        <label for="capture" class="block text-sm font-medium text-storm mb-1">{{ $t('config.capture') }}</label>
        <select id="capture" class="w-full bg-deep border border-storm rounded-lg px-3 py-2 text-silver focus:border-ice focus:outline-none" v-model="config.capture">
          <option value="">{{ $t('_common.autodetect') }}</option>
          <PlatformLayout :platform="platform">
            <template #linux>
              <option value="nvfbc">NvFBC</option>
              <option value="wlr">wlroots</option>
              <option value="kms">KMS</option>
              <option value="x11">X11</option>
            </template>
            <template #windows>
              <option value="ddx">Desktop Duplication API</option>
              <option value="wgc">Windows.Graphics.Capture {{ $t('_common.beta') }}</option>
            </template>
          </PlatformLayout>
        </select>
        <div class="text-sm text-storm mt-1">{{ $t('config.capture_desc') }}</div>
      </div>

      <div class="mb-3">
        <label for="encoder" class="block text-sm font-medium text-storm mb-1">{{ $t('config.encoder') }}</label>
        <select id="encoder" class="w-full bg-deep border border-storm rounded-lg px-3 py-2 text-silver focus:border-ice focus:outline-none" v-model="config.encoder">
          <option value="">{{ $t('_common.autodetect') }}</option>
          <PlatformLayout :platform="platform">
            <template #windows>
              <option value="nvenc">NVIDIA NVENC</option>
              <option value="quicksync">Intel QuickSync</option>
              <option value="amdvce">AMD AMF/VCE</option>
            </template>
            <template #linux>
              <option value="nvenc">NVIDIA NVENC</option>
              <option value="vaapi">VA-API</option>
            </template>
            <template #macos>
              <option value="videotoolbox">VideoToolbox</option>
            </template>
          </PlatformLayout>
          <option value="software">{{ $t('config.encoder_software') }}</option>
        </select>
        <div class="text-sm text-storm mt-1">{{ $t('config.encoder_desc') }}</div>
      </div>
      </div>
    </details>

    <details class="settings-section settings-section-compact settings-disclosure">
      <summary class="settings-disclosure-summary">
        <div>
          <div class="section-kicker">Launcher migration</div>
          <h3 class="settings-section-title mt-2">Compositor handoff for Steam, Heroic, and Lutris</h3>
          <div class="settings-summary-copy">When a launcher is already running on the desktop compositor, game launch IPC calls go to the existing process and games open on the wrong display. These options migrate the launcher into the stream session and optionally restore it afterward.</div>
        </div>
        <svg class="settings-disclosure-chevron h-4 w-4 text-storm" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 9-7 7-7-7" /></svg>
      </summary>

      <div class="settings-disclosure-body">
        <div class="grid gap-3 xl:grid-cols-2">
          <div class="surface-muted p-4">
            <div class="text-sm font-medium text-silver">Migrate launcher to stream compositor</div>
            <div class="mt-1 text-sm text-storm">Shut down the launcher before the private compositor starts so it relaunches inside the stream session.</div>
            <div class="mt-3 rounded bg-deep/60 px-2 py-1 font-mono text-xs text-storm">linux_cage_migrate_launcher</div>
            <label class="mt-4 flex items-center justify-between gap-4">
              <span class="text-xs uppercase tracking-[0.18em] text-storm">On stream start</span>
              <input
                type="checkbox"
                class="sr-only peer"
                :checked="config.linux_cage_migrate_launcher === 'enabled'"
                @change="config.linux_cage_migrate_launcher = $event.target.checked ? 'enabled' : 'disabled'"
              >
              <div class="relative h-5 w-9 rounded-full bg-storm/40 transition-colors peer-checked:bg-accent after:absolute after:left-[2px] after:top-[2px] after:h-4 after:w-4 after:rounded-full after:bg-white after:transition-all after:content-[''] peer-checked:after:translate-x-full"></div>
            </label>
          </div>

          <div class="surface-muted p-4">
            <div class="text-sm font-medium text-silver">Restore launcher after stream</div>
            <div class="mt-1 text-sm text-storm">After the stream session ends, relaunch Steam silently to the system tray on the desktop compositor. Only has effect when migration is also enabled.</div>
            <div class="mt-3 rounded bg-deep/60 px-2 py-1 font-mono text-xs text-storm">linux_cage_restore_launcher</div>
            <label class="mt-4 flex items-center justify-between gap-4">
              <span class="text-xs uppercase tracking-[0.18em] text-storm">On stream end</span>
              <input
                type="checkbox"
                class="sr-only peer"
                :checked="config.linux_cage_restore_launcher === 'enabled'"
                @change="config.linux_cage_restore_launcher = $event.target.checked ? 'enabled' : 'disabled'"
              >
              <div class="relative h-5 w-9 rounded-full bg-storm/40 transition-colors peer-checked:bg-accent after:absolute after:left-[2px] after:top-[2px] after:h-4 after:w-4 after:rounded-full after:bg-white after:transition-all after:content-[''] peer-checked:after:translate-x-full"></div>
            </label>
          </div>
        </div>
      </div>
    </details>
  </div>
</template>

<style scoped>

</style>
