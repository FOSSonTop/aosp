---
# https://vitepress.dev/reference/default-theme-home-page
layout: home

hero:
  name: "FOSSonTop"
  text: "AOSP Docs"
  actions:
    - theme: brand
      text: Add Content to Docs
      link: https://github.com/FOSSonTop/aosp
    - theme: alt
      text: Go Back
      link: ../index
features:
  - title: Build Guide
    details: Simple Android Build Guide with General steps to get started.
    link: ./building-101
  - title: Build Legacy
    details: Guide to build legacy android versions.
    link: ./legacy/build-legacy-android
  - title: Build for Ultra Legacy
    details: Guide to build newer versions for legacy devices.
    link: ./legacy/build-for-old-devices
  - title: Information And Guides
    details: Simple Index of Guides and tidbits
    link: ./info
  - title: Remote Build Execution Guide
    details: A guide on how to use reclient for remote builds and remote caching on lower end devices
    link: ./remote-build-execution
  - title: Get Help
    details: Simple Index of places where you can try going to when you need help.
    link: ./info/get-help
#features:
#  - icon: We'll fill these later as we're doing the migration
#    title: 
#    link: 
---

To build android, you need a server. Often times, these are paid.

Here are some options:

- foss.crave.io (free)
- Google Cloud (paid, with free trial)

