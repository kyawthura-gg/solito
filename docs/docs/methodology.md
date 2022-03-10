---
title: Methodology
---

With gradual adoption in mind, it's useful to know the Solito methodology to navigation. You might be wondering, where am I sharing code here, and where do I write different code per-platform?

The answer: the code for screens is 100% shared. So is the code for navigating **between** screens.

So, where is code _not_ shared? Whenever screens themeselves are <u>rendered</u>.

Solito views your screens as primitives that make up your app (and website). It's up to your platform to implement how those screens are displayed. The platform is like the skeleton, ready to render your screens however it wants.

On iOS and Android, you'll implement these shared screens using stacks, tabs, and drawers from React Navigation. You have full freedom to implement these. To a React Native developer, this should be completely familiar.

Meanwhile, on Next.js, you will render your shared screen components inside of your `pages` folder like always.

To recap, the `<ArtistScreen />` component is the same on iOS, Android and Web. The difference is, your Next.js website renders that via `/pages/artist/[slug]`, whereas the native app renders it via `<Screen component={ArtistScreen} />` from React Navigation.

So, how is this possible? Do the Next.js site and React Native app have to "communicate" somehow? How can `<Link href="/users/fernando" />` open the same screen on both platforms?

Solito treats URLs as your source of truth. Your Next.js app and React Native app don't communicate at all. They live in total isolation. <u>**Solito works by doing this: give me a URL, I'll detect what platform you're using, and then I'll figure out how to navigate to that URL.**</u>

Lets call this [headless navigation](https://github.com/axeldelafosse/expo-next-monorepo-example/pull/1#issuecomment-1005969004). Your [screens are your primitives](https://github.com/axeldelafosse/expo-next-monorepo-example/pull/1#issuecomment-1009185655), and they're fully shared across platforms. It's then up to each platform to implement these screens, and ensure that the URLs across platforms map to the same screens.

In the future, React Native may have a [`pages` folder API](https://github.com/EvanBacon/expo-auto-navigation-webpack), like Next.js. This would be the final missing piece for true code sharing.

## Web vs. Native patterns

It's worth noting the differences between Web and Native navigation patterns. Web navigation is flat: you have one screen mounted at a time. When you change screens, the current one unmounts, and a new one opens. On native, this is not the case. Native screens animate in their transitions from one screen to another.

If you change tabs on a native app, the previous tab remains mounted, preserving its scroll position and state. You often have shared headers across pages. These subtle differences are important to preserve. Cross-platform apps often get a bad rap because they try to make the user experience the exact same on every platform. But this is not the goal. <u>**We need to match our users' expecations based on the platform they're using.**</u>

By using URLs as our source of truth for firing page changes, Solito doesn't get in the way of how you implement your screens. It lets your website have a completely different header, footer, and sidebar UI than your native app. I'll repeat it again: your screens are your primitives, and your platform is your skeleton. In this case, your Web `Layout` component never gets used on iOS/Android, unless you want it to.

| [BeatGig Website](https://beatgig.com/search) | [BeatGig iOS App](https://apps.apple.com/us/app/beatgig/id1355182285?platform=iphone) |
| --------------------------------------------- | ------------------------------------------------------------------------------------- |
| <img src="/img/site.jpg" />                   | <img src="/img/app.PNG" />                                                            |

While you share your screen code, the skeleton of your product can differ by platform. Notice that the artists list is the same, but the naivgation differs between the Website and iOS app. This is the proper abstraction. If you want to make them look identical, you can. But you aren't forced to.

## Should I use Solito?

If you're building a Next.js app, there's no _not_ to use Solito. After all, it provides all the functionality that you're already getting from `useRouter` and `Link`, plus some useful utilities like `useParam`. Plus, any native-only React Navigation code is tree shaken.

If you're building a React Native app with React Navigation, your app can be spun into a website in like a day or two. You should use the web's URL mental model to get around screens, rather than screen names. If you use Solito to get around screens and read in `params`, then the moment you want to turn your app into a Next.js site, the hard parts are already done.

And so, my short answer is, why not use Solito? If you think you'll ever use React Native, or React Native Web for your website (just like [twitter.com](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2019/buildingfasterwithcomponents)), then Solito is future-proof. React Native is my go-to framework, [even if I'm only building a website](https://www.youtube.com/watch?v=0lnbdRweJtA&t=22s). And Solito glues it all together.

## Context

My thinking for this methodology was first documented and discussed by the community [here](https://github.com/axeldelafosse/expo-next-monorepo-example/pull/1#issuecomment-1005969004).