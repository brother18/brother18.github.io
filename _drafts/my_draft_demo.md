EventBus
========
EventBus is an Android optimized publish/subscribe event bus. A typical use case for Android apps is gluing Activities, Fragments, and background threads together. Conventional wiring of those elements often introduces complex and error-prone dependencies and life cycle issues. With EventBus propagating listeners through all participants (e.g. background service -> activity -> multiple fragments or helper classes) becomes deprecated. EventBus decouples event senders and receivers and thus simplifies communication between app components. Less code, better quality. And you don't need to implement a single interface!

