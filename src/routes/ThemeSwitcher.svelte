<script>
// @ts-nocheck

import { onMount } from 'svelte'

onMount(async() => {
  /*!
  * Color mode toggler for Bootstrap's docs (https://getbootstrap.com/)
  * Copyright 2011-2023 The Bootstrap Authors
  * Licensed under the Creative Commons Attribution 3.0 Unported License.
  */

  (() => {
    'use strict'

    const getStoredTheme = () => localStorage.getItem('theme')
    const setStoredTheme = theme => localStorage.setItem('theme', theme)

    const getPreferredTheme = () => {
      const storedTheme = getStoredTheme()
      if (storedTheme) {
        return storedTheme
      }

      return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light'
    }

    // @ts-ignore
    const setTheme = theme => {
      if (theme === 'auto' && window.matchMedia('(prefers-color-scheme: dark)').matches) {
        document.documentElement.setAttribute('data-bs-theme', 'dark')
      } else {
        document.documentElement.setAttribute('data-bs-theme', theme)
      }
    }

    setTheme(getPreferredTheme())

    // @ts-ignore
    const showActiveTheme = (theme, focus = false) => {
      const themeSwitcher = document.querySelector('#bd-theme')

      if (!themeSwitcher) {
        return
      }

      const themeSwitcherText = document.querySelector('#bd-theme-text')
      const activeThemeIcon = document.querySelector('#theme-icon-active')
      const btnToActive = document.querySelector(`[data-bs-theme-value="${theme}"]`)
      // @ts-ignore
      const classOfActiveBtn = btnToActive.querySelector('.theme-icon').getAttribute('class')

      document.querySelectorAll('[data-bs-theme-value]').forEach(element => {
        element.classList.remove('active')
        element.setAttribute('aria-pressed', 'false')
      })

      // @ts-ignore
      btnToActive.classList.add('active')
      // @ts-ignore
      btnToActive.setAttribute('aria-pressed', 'true')
      // @ts-ignore
      activeThemeIcon.setAttribute('class', classOfActiveBtn)
      // @ts-ignore
      const themeSwitcherLabel = `${themeSwitcherText.textContent} (${btnToActive.dataset.bsThemeValue})`
      themeSwitcher.setAttribute('aria-label', themeSwitcherLabel)

      if (focus) {
        // @ts-ignore
        themeSwitcher.focus()
      }
    }

    window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', () => {
      const storedTheme = getStoredTheme()
      if (storedTheme !== 'light' && storedTheme !== 'dark') {
        setTheme(getPreferredTheme())
      }
    })

    showActiveTheme(getPreferredTheme())

    document.querySelectorAll('[data-bs-theme-value]')
      .forEach(toggle => {
        toggle.addEventListener('click', () => {
          const theme = toggle.getAttribute('data-bs-theme-value')
          setStoredTheme(theme)
          setTheme(theme)
          showActiveTheme(theme, true)
        })
      })
  })()
})
</script>

<style>
.active i.bi-check2 {
  display: block !important;
}
</style>

<li class="nav-item dropdown">
  <button class="btn btn-link nav-link py-2 px-0 px-lg-2 dropdown-toggle d-flex align-items-center" id="bd-theme" type="button" aria-expanded="false" data-bs-toggle="dropdown" data-bs-display="static" aria-label="Toggle theme (auto)">
    <i class="bi bi-circle-half theme-icon me-2" id="theme-icon-active"></i>
    <span class="d-lg-none ms-2" id="bd-theme-text">Toggle theme</span>
  </button>
  <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="bd-theme-text">
    <li>
      <button type="button" class="dropdown-item d-flex align-items-center" data-bs-theme-value="light" aria-pressed="false">
        <i class="bi bi-sun-fill theme-icon me-2"></i>
        Light
        <i class="bi ms-auto bi-check2 d-none"></i>
      </button>
    </li>
    <li>
      <button type="button" class="dropdown-item d-flex align-items-center" data-bs-theme-value="dark" aria-pressed="false">
        <i class="bi bi-moon-stars-fill theme-icon me-2"></i>
        <!-- <svg class="bi me-2 opacity-50 theme-icon"><use href="#moon-stars-fill"></use></svg> -->
        Dark
        <i class="bi ms-auto bi-check2 d-none"></i>
        <!-- <svg class="bi ms-auto d-none"><use href="#check2"></use></svg> -->
      </button>
    </li>
    <li>
      <button type="button" class="dropdown-item d-flex align-items-center active" data-bs-theme-value="auto" aria-pressed="true">
        <i class="bi bi-circle-half theme-icon me-2"></i>
        <!-- <svg class="bi me-2 opacity-50 theme-icon"><use href="#circle-half"></use></svg> -->
        Auto
        <i class="bi ms-auto bi-check2 d-none"></i>
        <!-- <svg class="bi ms-auto d-none"><use href="#check2"></use></svg> -->
      </button>
    </li>
  </ul>
</li>