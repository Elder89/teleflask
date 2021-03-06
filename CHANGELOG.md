# Changelog
## v1.0.0
- Not any longer subclasses `flask.Flask`. This was ugly, and bad.
    Now you initialize it like this:
    ```python    
    bot = Teleflask(API_KEY, app)
    ```
    or 
    ```python
    bot = Teleflask(API_KEY)
    bot.init_app(app)
    ```
    - renamed `TeleflaskComplete` to just `Teleflask`
    - Make it loadable via `.init_app(app)`
    
    - You can now import `Teleflask` from the root of the module directly.
        ```python
        from teleflask import Teleflask
        ```
    - Actual setting of the webhook can be disabled via `DISABLE_SETTING_TELEGRAM_WEBHOOK=True` in the flask config.
      This is probably only usefull for tests.

- Mixin overhaul
    - all
        - any list/dict used for storage is now defined in the `__init__` method, so that it won't be global part of the class any longer.
        - decorators where you can specify required params can now be used multible times to allow different fields to trigger the same function.
        - all listeners which depend on incomming `update`s will now always have the `update` as first parameter.

    - `StartupMixin`
        - Added `__init__` method to `StartupMixin`, else the lists were static.
        - Added unit testing of `StartupMixin`.

    - `UpdatesMixin` overhaul
        - Added `__init__` method to `UpdatesMixin`, else the dict were static.
        - Changed dict to be OrderedDict, to preverse order on pre 3.6 systems.
        - Fixed `@on_update` did not return the (new) function.
        - Changed `add_update_listener` to merge keywords.
            - this is relevant for `@on_update`, too.
        - Added unit tests for `UpdatesMixin`.
    - `MessagesMixin`
        - Fixed `@on_message` did not return the (new) function.
        - `@on_message` will now really provide the `update` as fist argument.
            This is now conform to all other listeners, and a part of already existing documentation.

- Fixes in `messages`:
    - Let `MessageWithReplies` also return the results.
    - Allow `TypingMessage` to use the `TypingMessage.CANCEL`.
    - Fixed `DocumentMessage` to respect the set `file_mime`, and in case of given `file_content`, use the filename part from either the `file_path` or the `file_url`.
    - Also unknown `PhotoMessage`s will now have a `*.unknown-file-type.png` suffix.
    - Fixed `teleflask.server.base.TeleflaskBase.process_result`, now also setting `reply_to` and `reply_id` for edits and in channels.

- specified minimum versions for some dependencies
    - `pytgbot>=2.3.3` (for the new webhooks)
    - `luckydonald-utils>=0.52` (needed `cut_paragraphs` in `messages.py`)
    - `backoff>=1.4.1` (for the example using the flask development server, see [backoff#30](https://github.com/litl/backoff/issues/30))
        
