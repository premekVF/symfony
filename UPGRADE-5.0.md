UPGRADE FROM 4.x to 5.0
=======================

BrowserKit...
----------

 * Removed `Client`, use `AbstractBrowser` instead
 * Removed the possibility to extend `Response` by making it final.
 * Removed `Response::buildHeader()`
 * Removed `Response::getStatus()`, use `Response::getStatusCode()` instead
 * The `Client::submit()` method has a new `$serverParameters` argument.

Cache
-----

 * Removed `CacheItem::getPreviousTags()`, use `CacheItem::getMetadata()` instead.
 * Removed all PSR-16 adapters, use `Psr16Cache` or `Symfony\Contracts\Cache\CacheInterface` implementations instead.
 * Removed `SimpleCacheAdapter`, use `Psr16Adapter` instead.
 * Added argument `$prefix` to `AdapterInterface::clear()`

Config
------

 * Dropped support for constructing a `TreeBuilder` without passing root node information.
 * Added the `getChildNodeDefinitions()` method to `ParentNodeDefinitionInterface`.
 * The `Processor` class has been made final
 * Removed `FileLoaderLoadException`, use `LoaderLoadException` instead.
 * Using environment variables with `cannotBeEmpty()` if the value is validated with `validate()` will throw an exception.
 * Removed the `root()` method in `TreeBuilder`, pass the root node information to the constructor instead

Console
-------

 * Removed the `setCrossingChar()` method in favor of the `setDefaultCrossingChar()` method in `TableStyle`.
 * Removed the `setHorizontalBorderChar()` method in favor of the `setDefaultCrossingChars()` method in `TableStyle`.
 * Removed the `getHorizontalBorderChar()` method in favor of the `getBorderChars()` method in `TableStyle`.
 * Removed the `setVerticalBorderChar()` method in favor of the `setVerticalBorderChars()` method in `TableStyle`.
 * Removed the `getVerticalBorderChar()` method in favor of the `getBorderChars()` method in `TableStyle`.
 * The `ProcessHelper::run()` method takes the command as an array of arguments.

   Before:
   ```php
   $processHelper->run($output, 'ls -l');
   ```

   After:
   ```php
   $processHelper->run($output, array('ls', '-l'));

   // alternatively, when a shell wrapper is required
   $processHelper->run($output, Process::fromShellCommandline('ls -l'));
   ```

DependencyInjection
-------------------

 * Removed the `TypedReference::canBeAutoregistered()` and  `TypedReference::getRequiringClass()` methods.
 * Removed support for auto-discovered extension configuration class which does not implement `ConfigurationInterface`.
 * Removed support for non-string default env() parameters

   Before:
   ```yaml
   parameters:
        env(NAME): 1.5
   ```

   After:
   ```yaml
   parameters:
        env(NAME): '1.5'
   ```

 * Removed support for short factories and short configurators in Yaml

   Before:
   ```yaml
   services:
     my_service:
       factory: factory_service:method
   ```

   After:
   ```yaml
   services:
     my_service:
       factory: ['@factory_service', method]
   ```
 * Removed `tagged` in favor of `tagged_iterator`

   Before:
   ```yaml
   services:
       App\Handler:
            tags: ['app.handler']

        App\HandlerCollection:
            arguments: [!tagged app.handler]
   ```

   After:
   ```yaml
   services:
       App\Handler:
           tags: ['app.handler']

       App\HandlerCollection:
           arguments: [!tagged_iterator app.handler]
   ```

DoctrineBridge
--------------

 * Deprecated injecting `ClassMetadataFactory` in `DoctrineExtractor`, an instance of `EntityManagerInterface` should be
   injected instead
 * Passing an `IdReader` to the `DoctrineChoiceLoader` when the query cannot be optimized with single id field will throw an exception, pass `null` instead
 * Not passing an `IdReader` to the `DoctrineChoiceLoader` when the query can be optimized with single id field will not apply any optimization

DomCrawler
----------

 * The `Crawler::children()` method has a new `$selector` argument.

Dotenv
------

 * First parameter `$usePutenv` of `Dotenv::__construct()` now default to `false`.

EventDispatcher
---------------

 * The `TraceableEventDispatcherInterface` has been removed.
 * The signature of the `EventDispatcherInterface::dispatch()` method has been updated to `dispatch($event, string $eventName = null)`
 * The `Event` class has been removed, use `Symfony\Contracts\EventDispatcher\Event` instead

Filesystem
----------

 * The `Filesystem::isAbsolutePath()` method no longer supports `null` in the `$file` argument.
 * The `Filesystem::dumpFile()` method no longer supports arrays in the `$content` argument.
 * The `Filesystem::appendToFile()` method no longer supports arrays in the `$content` argument.

Finder
------

 * The `Finder::sortByName()` method has a new `$useNaturalSort` argument.

Form
----

 * Removed support for using `int` or `float` as data for the `NumberType` when the `input` option is set to `string`.
 * Removed support for using the `format` option of `DateType` and `DateTimeType` when the `html5` option is enabled.
 * Using names for buttons that do not start with a letter, a digit, or an underscore leads to an exception.
 * Using names for buttons that do not contain only letters, digits, underscores, hyphens, and colons leads to an
   exception.
 * Using the `date_format`, `date_widget`, and `time_widget` options of the `DateTimeType` when the `widget` option is
   set to `single_text` is not supported anymore.
 * The `getExtendedType()` method was removed from the `FormTypeExtensionInterface`. It is replaced by the the static
   `getExtendedTypes()` method which must return an iterable of extended types.

   Before:

   ```php
   class FooTypeExtension extends AbstractTypeExtension
   {
       public function getExtendedType()
       {
           return FormType::class;
       }

       // ...
   }
   ```

   After:

   ```php
   class FooTypeExtension extends AbstractTypeExtension
   {
       public static function getExtendedTypes(): iterable
       {
           return array(FormType::class);
       }

       // ...
   }
   ```
 * The `scale` option was removed from the `IntegerType`.
 * The `$scale` argument of the `IntegerToLocalizedStringTransformer` was removed.
 * Calling `FormRenderer::searchAndRenderBlock` for fields which were already rendered
   throws an exception instead of returning empty strings:

   Before:
   ```twig
   {% for field in fieldsWithPotentialDuplicates %}
      {{ form_widget(field) }}
   {% endfor %}
   ```

   After:
   ```twig
   {% for field in fieldsWithPotentialDuplicates if not field.rendered %}
      {{ form_widget(field) }}
   {% endfor %}
   ```

 * The `regions` option was removed from the `TimezoneType`.

FrameworkBundle
---------------

 * Dropped support for booting the kernel before running `WebTestCase::createClient()`. `createClient()` will throw an
   exception if the kernel was already booted before.
 * Removed the `framework.templating` option, use Twig instead.
 * The project dir argument of the constructor of `AssetsInstallCommand` is required.
 * Removed support for `bundle:controller:action` syntax to reference controllers. Use `serviceOrFqcn::method`
   instead where `serviceOrFqcn` is either the service ID when using controllers as services or the FQCN of the controller.

   Before:

   ```yml
   bundle_controller:
       path: /
       defaults:
           _controller: FrameworkBundle:Redirect:redirect
   ```

   After:

   ```yml
   bundle_controller:
       path: /
       defaults:
           _controller: Symfony\Bundle\FrameworkBundle\Controller\RedirectController::redirectAction
   ```

 * Removed `Symfony\Bundle\FrameworkBundle\Controller\ControllerNameParser`.
 * Warming up a router in `RouterCacheWarmer` that does not implement the `WarmableInterface` is not supported anymore.
 * The `RequestDataCollector` class has been removed. Use the `Symfony\Component\HttpKernel\DataCollector\RequestDataCollector` class instead.
 * Removed `Symfony\Bundle\FrameworkBundle\Controller\Controller`. Use `Symfony\Bundle\FrameworkBundle\Controller\AbstractController` instead.
 * Added support for the SameSite attribute for session cookies. It is highly recommended to set this setting (`framework.session.cookie_samesite`) to `lax` for increased security against CSRF attacks.
 * The `ContainerAwareCommand` class has been removed, use `Symfony\Component\Console\Command\Command`
   with dependency injection instead.
 * The `Templating\Helper\TranslatorHelper::transChoice()` method has been removed, use the `trans()` one instead with a `%count%` parameter.
 * Removed support for legacy translations directories `src/Resources/translations/` and `src/Resources/<BundleName>/translations/`, use `translations/` instead.
 * Support for the legacy directory structure in `translation:update` and `debug:translation` commands has been removed.
 * Removed the "Psr\SimpleCache\CacheInterface" / "cache.app.simple" service, use "Symfony\Contracts\Cache\CacheInterface" / "cache.app" instead.
 * Removed support for `templating` engine in `TemplateController`, use Twig instead

HttpClient
----------

 * Added method `cancel()` to `ResponseInterface`
 * The `$parser` argument of `ControllerResolver::__construct()` and `DelegatingLoader::__construct()`
   has been removed.
 * The `ControllerResolver` and `DelegatingLoader` classes have been made `final`.
 * The `controller_name_converter` and `resolve_controller_name_subscriber` services have been removed.

HttpFoundation
--------------

 * The `$size` argument of the `UploadedFile` constructor has been removed.
 * The `getClientSize()` method of the `UploadedFile` class has been removed.
 * The `getSession()` method of the `Request` class throws an exception when session is null.
 * The default value of the "$secure" and "$samesite" arguments of Cookie's constructor
   changed respectively from "false" to "null" and from "null" to "lax".
 * The `MimeTypeGuesserInterface` and `ExtensionGuesserInterface` interfaces have been removed,
   use `Symfony\Component\Mime\MimeTypesInterface` instead.
 * The `MimeType` and `MimeTypeExtensionGuesser` classes have been removed,
   use `Symfony\Component\Mime\MimeTypes` instead.
 * The `FileBinaryMimeTypeGuesser` class has been removed,
   use `Symfony\Component\Mime\FileBinaryMimeTypeGuesser` instead.
 * The `FileinfoMimeTypeGuesser` class has been removed,
   use `Symfony\Component\Mime\FileinfoMimeTypeGuesser` instead.
 * `ApacheRequest` has been removed, use the `Request` class instead.

HttpKernel
----------

 * The `getPublicDir()` method has been added to the `BundleInterface`.
 * Removed `Client`, use `HttpKernelBrowser` instead
 * The `Kernel::getRootDir()` and the `kernel.root_dir` parameter have been removed
 * The `KernelInterface::getName()` and the `kernel.name` parameter have been removed
 * Removed the first and second constructor argument of `ConfigDataCollector`
 * Removed `ConfigDataCollector::getApplicationName()`
 * Removed `ConfigDataCollector::getApplicationVersion()`
 * Removed `FilterControllerArgumentsEvent`, use `ControllerArgumentsEvent` instead
 * Removed `FilterControllerEvent`, use `ControllerEvent` instead
 * Removed `FilterResponseEvent`, use `ResponseEvent` instead
 * Removed `GetResponseEvent`, use `RequestEvent` instead
 * Removed `GetResponseForControllerResultEvent`, use `ViewEvent` instead
 * Removed `GetResponseForExceptionEvent`, use `ExceptionEvent` instead
 * Removed `PostResponseEvent`, use `TerminateEvent` instead
 * Removed `TranslatorListener` in favor of `LocaleAwareListener`
 * The `DebugHandlersListener` class has been made `final`

Intl
----

 * Removed `ResourceBundle` namespace
 * Removed `Intl::getLanguageBundle()`, use `Languages` or `Scripts` instead
 * Removed `Intl::getCurrencyBundle()`, use `Currencies` instead
 * Removed `Intl::getLocaleBundle()`, use `Locales` instead
 * Removed `Intl::getRegionBundle()`, use `Countries` instead

Lock
----

 * Removed `Symfony\Component\Lock\StoreInterface` in favor of `Symfony\Component\Lock\BlockingStoreInterface` and
   `Symfony\Component\Lock\PersistStoreInterface`.
 * Removed `Factory`, use `LockFactory` instead

Messenger
---------

 * The `LoggingMiddleware` class has been removed, pass a logger to `SendMessageMiddleware` instead.
 * Passing a `ContainerInterface` instance as first argument of the `ConsumeMessagesCommand` constructor now
   throws as `\TypeError`, pass a `RoutableMessageBus`  instance instead.

Monolog
-------

 * The methods `DebugProcessor::getLogs()`, `DebugProcessor::countErrors()`, `Logger::getLogs()` and `Logger::countErrors()` have a new `$request` argument.

MonologBridge
--------------

* The `RouteProcessor` class is final.

Process
-------

 * Removed the `Process::setCommandline()` and the `PhpProcess::setPhpBinary()` methods.
 * Commands must be defined as arrays when creating a `Process` instance.

   Before:
   ```php
   $process = new Process('ls -l');
   ```

   After:
   ```php
   $process = new Process(array('ls', '-l'));

   // alternatively, when a shell wrapper is required
   $process = Process::fromShellCommandline('ls -l');
   ```

PropertyAccess
--------------

 * Removed support of passing `null` as 2nd argument of
   `PropertyAccessor::createCache()` method (`$defaultLifetime`), pass `0`
   instead.

Routing
-------

 * The `generator_base_class`, `generator_cache_class`, `matcher_base_class`, and `matcher_cache_class` router
   options have been removed.
 * `Serializable` implementing methods for `Route` and `CompiledRoute` are final.
   Instead of overwriting them, use `__serialize` and `__unserialize` as extension points which are forward compatible
   with the new serialization methods in PHP 7.4.

Security
--------

 * Implementations of `PasswordEncoderInterface` and `UserPasswordEncoderInterface` must have a new `needsRehash()` method
 * The `Role` and `SwitchUserRole` classes have been removed.
 * The `getReachableRoles()` method of the `RoleHierarchy` class has been removed. It has been replaced by the new
   `getReachableRoleNames()` method.
 * The `getRoles()` method has been removed from the `TokenInterface`. It has been replaced by the new
   `getRoleNames()` method.
 * The `ContextListener::setLogoutOnUserChange()` method has been removed.
 * The `Symfony\Component\Security\Core\User\AdvancedUserInterface` has been removed.
 * The `ExpressionVoter::addExpressionLanguageProvider()` method has been removed.
 * The `FirewallMapInterface::getListeners()` method must return an array of 3 elements,
   the 3rd one must be either a `LogoutListener` instance or `null`.
 * The `AuthenticationTrustResolver` constructor arguments have been removed.
 * A user object that is not an instance of `UserInterface` cannot be accessed from `Security::getUser()` anymore and returns `null` instead.
 * `SimpleAuthenticatorInterface`, `SimpleFormAuthenticatorInterface`, `SimplePreAuthenticatorInterface`,
   `SimpleAuthenticationProvider`, `SimpleAuthenticationHandler`, `SimpleFormAuthenticationListener` and
   `SimplePreAuthenticationListener` have been removed. Use Guard instead.
 * The `ListenerInterface` has been removed, turn your listeners into callables instead.
 * The `Firewall::handleRequest()` method has been removed, use `Firewall::callListeners()` instead.
 * `\Serializable` interface has been removed from `AbstractToken` and `AuthenticationException`,
   thus `serialize()` and `unserialize()` aren't available.
   Use `__serialize()` and `__unserialize()` instead.

   Before:
   ```php
   public function serialize()
   {
       return [$this->myLocalVar, parent::serialize()];
   }

   public function unserialize($serialized)
   {
       [$this->myLocalVar, $parentSerialized] = unserialize($serialized);
       parent::unserialize($parentSerialized);
   }
   ```

   After:
   ```php
   public function __serialize(): array
   {
       return [$this->myLocalVar, parent::__serialize()];
   }

   public function __unserialize(array $data): void
   {
       [$this->myLocalVar, $parentData] = $data;
       parent::__unserialize($parentData);
   }
   ```

 * The `Argon2iPasswordEncoder` class has been removed, use `SodiumPasswordEncoder` instead.
 * The `BCryptPasswordEncoder` class has been removed, use `NativePasswordEncoder` instead.
 * Classes implementing the `TokenInterface` must implement the two new methods
   `__serialize` and `__unserialize`

SecurityBundle
--------------

 * The `logout_on_user_change` firewall option has been removed.
 * The `switch_user.stateless` firewall option has been removed.
 * The `SecurityUserValueResolver` class has been removed.
 * Passing a `FirewallConfig` instance as 3rd argument to  the `FirewallContext` constructor
   now throws a `\TypeError`, pass a `LogoutListener` instance instead.
 * The `security.authentication.trust_resolver.anonymous_class` parameter has been removed.
 * The `security.authentication.trust_resolver.rememberme_class` parameter has been removed.
 * The `simple_form` and `simple_preauth` authentication listeners have been removed,
   use Guard instead.
 * The `SimpleFormFactory` and `SimplePreAuthenticationFactory` classes have been removed,
   use Guard instead.
 * The names of the cookies configured in the `logout.delete_cookies` option are
   no longer normalized. If any of your cookie names has dashes they won't be
   changed to underscores.
   Before: `my-cookie` deleted the `my_cookie` cookie (with an underscore).
   After: `my-cookie` deletes the `my-cookie` cookie (with a dash).
 * Configuring encoders using `argon2i` or `bcrypt` as algorithm is not supported anymore, use `auto` instead.

Serializer
----------

 * The `AbstractNormalizer::handleCircularReference()` method has two new `$format` and `$context` arguments.

Translation
-----------

 * The `FileDumper::setBackup()` method has been removed.
 * The `TranslationWriter::disableBackup()` method has been removed.
 * The `TranslatorInterface` has been removed in favor of `Symfony\Contracts\Translation\TranslatorInterface`
 * The `MessageSelector`, `Interval` and `PluralizationRules` classes have been removed, use `IdentityTranslator` instead
 * The `Translator::getFallbackLocales()` and `TranslationDataCollector::getFallbackLocales()` method are now internal
 * The `Translator::transChoice()` method has been removed in favor of using `Translator::trans()` with "%count%" as the parameter driving plurals

TwigBundle
----------

 * The default value (`false`) of the `twig.strict_variables` configuration option has been changed to `%kernel.debug%`.
 * The `transchoice` tag and filter have been removed, use the `trans` ones instead with a `%count%` parameter.
 * Removed support for legacy templates directories `src/Resources/views/` and `src/Resources/<BundleName>/views/`, use `templates/` and `templates/bundles/<BundleName>/` instead.

TwigBridge
----------

 * Removed argument `$rootDir` from the `DebugCommand::__construct()` method and the 5th argument must be an instance of `FileLinkFormatter`
 * removed the `$requestStack` and `$requestContext` arguments of the
   `HttpFoundationExtension`, pass a `Symfony\Component\HttpFoundation\UrlHelper`
   instance as the only argument instead

Validator
--------

 * Removed support for non-string codes of a `ConstraintViolation`. A `string` type-hint was added to the constructor of
   the `ConstraintViolation` class and to the `ConstraintViolationBuilder::setCode()` method.
 * An `ExpressionLanguage` instance or null must be passed as the first argument of `ExpressionValidator::__construct()`
 * The `checkMX` and `checkHost` options of the `Email` constraint were removed
 * The `Email::__construct()` 'strict' property has been removed. Use 'mode'=>"strict" instead.
 * Calling `EmailValidator::__construct()` method with a boolean parameter has been removed, use `EmailValidator("strict")` instead.
 * Removed the `checkDNS` and `dnsMessage` options from the `Url` constraint.
 * The component is now decoupled from `symfony/translation` and uses `Symfony\Contracts\Translation\TranslatorInterface` instead
 * The `ValidatorBuilderInterface` has been removed
 * Removed support for validating instances of `\DateTimeInterface` in `DateTimeValidator`, `DateValidator` and `TimeValidator`. Use `Type` instead or remove the constraint if the underlying model is type hinted to `\DateTimeInterface` already.
 * The `symfony/intl` component is now required for using the `Bic`, `Country`, `Currency`, `Language` and `Locale` constraints
 * The `egulias/email-validator` component is now required for using the `Email` constraint in strict mode
 * The `symfony/expression-language` component is now required for using the `Expression` constraint

WebProfilerBundle
-----------------

 * Removed the `ExceptionController::templateExists()` method
 * Removed the `TemplateManager::templateExists()` method

Workflow
--------

 * The `DefinitionBuilder::reset()` method has been removed, use the `clear()` one instead.
 * `add` method has been removed use `addWorkflow` method in `Workflow\Registry` instead.
 * `SupportStrategyInterface` has been removed, use `WorkflowSupportStrategyInterface` instead.
 * `ClassInstanceSupportStrategy` has been removed, use `InstanceOfSupportStrategy` instead.
 * `MarkingStoreInterface::setMarking()` has a third argument: `array $context = []`.
 * Removed support of `initial_place`. Use `initial_places` instead.
 * `MultipleStateMarkingStore` has been removed. Use `MethodMarkingStore` instead.
 * `DefinitionBuilder::setInitialPlace()` has been removed, use `DefinitionBuilder::setInitialPlaces()` instead.

   Before:
   ```yaml
   framework:
       workflows:
           type: workflow
           article:
               marking_store:
                   type: multiple
                   arguments: states
   ```

   After:
   ```yaml
   framework:
       workflows:
           type: workflow
           article:
               marking_store:
                   property: states
   ```
 * `SingleStateMarkingStore` has been removed. Use `MethodMarkingStore` instead.

   Before:
   ```yaml
   framework:
       workflows:
           article:
               marking_store:
                   arguments: state
   ```

   After:
   ```yaml
   framework:
       workflows:
           article:
               marking_store:
                   property: state
   ```

 * Support for using a workflow with a single state marking is dropped. Use a state machine instead.

   Before:
   ```yaml
   framework:
       workflows:
           article:
               type: workflow
               marking_store:
                   type: single_state
   ```

   After:
   ```yaml
   framework:
       workflows:
           article:
               type: state_machine
   ```

Yaml
----

 * The parser is now stricter and will throw a `ParseException` when a
   mapping is found inside a multi-line string.

WebServerBundle
---------------

 * The bundle has been removed.
