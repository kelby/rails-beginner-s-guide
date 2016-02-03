# Error & Exception

```
DeserializationError
SerializationError
```

```ruby
# js, get, not localhost
ActionController::InvalidCrossOriginRequest

ActionMailer::NonInferrableMailerError

DeliveryMailer RuntimeError

ActionView::MissingTemplate

DeliveryMethods NoMethodError

DeliveryMailer RuntimeError

ActionMailer NonInferrableMailerError

ActiveRecord::RecordNotFound

ActiveRecord::ActiveRecordError

ActionController::RoutingError

InvalidIdentifiersError
```


```ruby
ActiveModel::ValidationError
AbstractController::DoubleRenderError
AbstractController::Error
AbstractController::ActionNotFound
Mail::UnknownEncodingType
ActiveRecord::DeleteRestrictionError
ActiveRecord::AssociationNotFoundError
ActiveRecord::InverseOfAssociationNotFoundError
ActiveRecord::HasManyThroughAssociationNotFoundError
ActiveRecord::HasManyThroughAssociationPolymorphicSourceError
ActiveRecord::HasManyThroughAssociationPolymorphicThroughError
ActiveRecord::HasManyThroughAssociationPointlessSourceTypeError
ActiveRecord::HasOneThroughCantAssociateThroughCollection
ActiveRecord::HasOneAssociationPolymorphicThroughError
ActiveRecord::HasManyThroughSourceAssociationNotFoundError
ActiveRecord::ThroughCantAssociateThroughHasOneOrManyReflection
ActiveRecord::HasManyThroughCantAssociateThroughHasOneOrManyReflection
ActiveRecord::HasOneThroughCantAssociateThroughHasOneOrManyReflection
ActiveRecord::HasManyThroughCantAssociateNewRecords
ActiveRecord::HasManyThroughCantDissociateNewRecords
ActiveRecord::ThroughNestedAssociationsAreReadonly
ActiveRecord::HasManyThroughNestedAssociationsAreReadonly
ActiveRecord::HasOneThroughNestedAssociationsAreReadonly
ActiveRecord::EagerLoadPolymorphicError
ActiveRecord::ReadOnlyAssociation
ActiveRecord::NestedAttributes::TooManyRecords

Concurrent::PromiseExecutionError

ActionController::ActionControllerError
ActionController::BadRequest
ActionController::RenderError
ActionController::RoutingError
ActionController::UrlGenerationError
ActionController::MethodNotAllowed
ActionController::NotImplemented
ActionController::UnknownController
ActionController::MissingFile
ActionController::SessionOverflowError
ActionController::UnknownHttpMethod
ActionController::UnknownFormat

ActiveModel::ForbiddenAttributesError

ActiveModel::StrictValidationFailed
ActiveModel::UnknownAttributeError
ActiveRecord::AttributeAssignmentError
ActiveRecord::IrreversibleMigration
ActiveRecord::DuplicateMigrationVersionError
ActiveRecord::DuplicateMigrationNameError
ActiveRecord::UnknownMigrationVersionError
ActiveRecord::IllegalMigrationNameError
ActiveRecord::PendingMigrationError
ActiveRecord::ConcurrentMigrationError

ActionDispatch::IllegalStateError

ActiveRecord::RecordInvalid
Mail::Field::FieldError
Mail::Field::ParseError
Mail::Field::SyntaxError

SignedGlobalID::ExpiredMessage

ActionController::ParameterMissing
ActionController::UnpermittedParameters
Rack::Test::Error
ActiveSupport::DeprecationException

ActiveSupport::Concern::MultipleIncludedBlocks
Bundler::Dsl::DSLError
ActiveSupport::MessageVerifier::InvalidSignature
I18n::ArgumentError
I18n::InvalidLocale
I18n::InvalidLocaleData
I18n::MissingTranslationData
I18n::InvalidPluralizationData
I18n::MissingInterpolationArgument
I18n::ReservedInterpolationKey

ActiveModel::MissingAttributeError
Arel::Visitors::UnsupportedVisitError
JSON::JSONError
Rake::CommandLineOptionError
Rack::QueryParser::ParameterTypeError
Rack::QueryParser::InvalidParameterError
ActionController::Live::ClientDisconnected

ActionDispatch::RemoteIp::IpSpoofAttackError
ActionView::Helpers::NumberHelper::InvalidNumberError
Sprockets::Rails::Helper::AssetNotPrecompiled
MIME::Type::InvalidContentType
MIME::Type::InvalidEncoding
Erubis::ErubisError
Erubis::NotSupportedError

# 配置一下 session store
ActionDispatch::Cookies::CookieOverflow


ActiveRecord::ConnectionTimeoutError
ActiveRecord::ExclusiveConnectionTimeoutError
MultiJson::AdapterError
MultiJson::ParseError
ActiveSupport::SafeBuffer::SafeConcatError
Nokogiri::XML::XPath::SyntaxError
Nokogiri::HTML::Document::EncodingFound
Puma::UnsupportedOption
Exception2MessageMapper::ErrNotRegisteredException
TSort::Cyclic
I18n::UnknownFileType
ActionDispatch::Journey::Router::RoutingError
StringScanner::Error
ActiveSupport::XMLConverter::DisallowedType
ActiveRecord::ActiveRecordError
ActiveRecord::SubclassNotFound
ActiveRecord::AssociationTypeMismatch
ActiveRecord::SerializationTypeMismatch
ActiveRecord::AdapterNotSpecified
ActiveRecord::AdapterNotFound
ActiveRecord::ConnectionNotEstablished
ActiveRecord::RecordNotFound
ActiveRecord::RecordNotSaved
ActiveRecord::RecordNotDestroyed
ActiveRecord::StatementInvalid
ActiveRecord::WrappedDatabaseException
ActiveRecord::RecordNotUnique
ActiveRecord::InvalidForeignKey
ActiveRecord::PreparedStatementInvalid
ActiveRecord::NoDatabaseError
ActiveRecord::StaleObjectError
ActiveRecord::ConfigurationError
ActiveRecord::ReadOnlyRecord
ActiveRecord::Rollback
ActiveRecord::DangerousAttributeError

ActiveRecord::MultiparameterAssignmentErrors
ActiveRecord::UnknownPrimaryKey
ActiveRecord::ImmutableRelation
ActiveRecord::TransactionIsolationError
ActiveRecord::MigrationError

WebConsole::Error
WebConsole::DoubleRenderError

Module::DelegationError
ActionDispatch::Session::SessionRestoreError
ActiveRecord::TypeConflictError
```

其它：

```ruby
# identified_by 里面声明一下。
ActionCable UnauthorizedError


```
