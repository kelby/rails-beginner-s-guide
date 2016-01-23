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

NameError

Exception




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

Loofah::ScrubberNotFound
ActiveModel::ForbiddenAttributesError
Puma::ConnectionError

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
OptionParser::ParseError
OptionParser::AmbiguousOption
OptionParser::NeedlessArgument
OptionParser::MissingArgument
OptionParser::InvalidOption
OptionParser::InvalidArgument
OptionParser::AmbiguousArgument

SocketError

ActionDispatch::IllegalStateError
TZInfo::InvalidCountryCode

Timeout::Error

TZInfo::NoOffsetsDefined
ActiveRecord::RecordInvalid
Mail::Field::FieldError
Mail::Field::ParseError
Mail::Field::SyntaxError

Concurrent::Transaction::AbortError
Concurrent::Transaction::LeaveError
Concurrent::Agent::Error
Concurrent::Agent::ValidationError

SignedGlobalID::ExpiredMessage
Sprockets::Error
Sprockets::ArgumentError
Sprockets::ContentTypeMismatch
Sprockets::NotImplementedError
Sprockets::NotFound
Sprockets::ConversionError
Sprockets::FileNotFound
Sprockets::FileOutsidePaths
Jbuilder::NullError
Jbuilder::ArrayError
ActionController::ParameterMissing
ActionController::UnpermittedParameters
Rack::Test::Error
ActiveSupport::DeprecationException
Logger::Error
Logger::ShiftingError
TZInfo::AmbiguousTime
TZInfo::PeriodNotFound
TZInfo::InvalidTimezoneIdentifier
TZInfo::UnknownTimezone
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
Racc::ParseError
Gem::VerificationError
Gem::UnsatisfiableDependencyError
TZInfo::InvalidDataSource
TZInfo::DataSourceNotFound
ActionDispatch::RemoteIp::IpSpoofAttackError
ActionView::Helpers::NumberHelper::InvalidNumberError
Sprockets::Rails::Helper::AssetNotPrecompiled
MIME::Type::InvalidContentType
MIME::Type::InvalidEncoding
Erubis::ErubisError
Erubis::NotSupportedError

Concurrent::ConcurrentUpdateError
ActionDispatch::Cookies::CookieOverflow
Concurrent::Error
Concurrent::ConfigurationError
Concurrent::CancelledOperationError
Concurrent::LifecycleError
Concurrent::ImmutabilityError
Concurrent::IllegalOperationError
Concurrent::InitializationError
Concurrent::MaxRestartFrequencyError
Concurrent::MultipleAssignmentError
Concurrent::RejectedExecutionError
Concurrent::ResourceLimitError
Concurrent::TimeoutError

Nokogiri::SyntaxError
Nokogiri::XML::SyntaxError
URI::GID::MissingModelIdError
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
JSON::ParserError
JSON::NestingError
JSON::CircularDatastructure
JSON::GeneratorError
JSON::MissingUnicodeSupport
WebConsole::Error
WebConsole::DoubleRenderError

Module::DelegationError
ActionDispatch::Session::SessionRestoreError
ActiveRecord::TypeConflictError

Psych::Exception
Psych::BadAlias
Psych::DisallowedClass
Psych::SyntaxError
URI::Error
URI::InvalidURIError
URI::InvalidComponentError
URI::BadURIError
Nokogiri::CSS::Tokenizer::ScanError
Nokogiri::CSS::SyntaxError

TypeError
ArgumentError
IndexError
KeyError
RangeError
NameError
NoMethodError
RuntimeError
EncodingError
Encoding::CompatibilityError
SystemCallError
UncaughtThrowError
ZeroDivisionError
FloatDomainError
Errno::NOERROR
Errno::EPERM
Errno::ENOENT
Errno::ESRCH
Errno::EINTR
Errno::EIO
Errno::ENXIO
Errno::E2BIG
Errno::ENOEXEC
Errno::EBADF
Errno::ECHILD
Errno::EAGAIN
Errno::ENOMEM
Errno::EACCES
Errno::EFAULT
Errno::ENOTBLK
Errno::EBUSY
Errno::EEXIST
Errno::EXDEV
Errno::ENODEV
Errno::ENOTDIR
Errno::EISDIR
Errno::EINVAL
Errno::ENFILE
Errno::EMFILE
Errno::ENOTTY
Errno::ETXTBSY
Errno::EFBIG
Errno::ENOSPC
Errno::ESPIPE
Errno::EROFS
Errno::EMLINK
Errno::EPIPE
Errno::EDOM
Errno::ERANGE
Errno::EDEADLK
Errno::ENAMETOOLONG
Errno::ENOLCK
Errno::ENOSYS
Errno::ENOTEMPTY
Errno::ELOOP
Errno::ENOMSG
Errno::EIDRM
Errno::ENOSTR
Errno::ENODATA
Errno::ETIME
Errno::ENOSR
Errno::EREMOTE
Errno::ENOLINK
Errno::EPROTO
Errno::EMULTIHOP
Errno::EBADMSG
Errno::EOVERFLOW
Errno::EILSEQ
Errno::EUSERS
Errno::ENOTSOCK
Errno::EDESTADDRREQ
Errno::EMSGSIZE
Errno::EPROTOTYPE
Errno::ENOPROTOOPT
Errno::EPROTONOSUPPORT
Errno::ESOCKTNOSUPPORT
Errno::EOPNOTSUPP
Errno::EPFNOSUPPORT
Errno::EAFNOSUPPORT
Errno::EADDRINUSE
Errno::EADDRNOTAVAIL
Errno::ENETDOWN
Errno::ENETUNREACH
Errno::ENETRESET
Errno::ECONNABORTED
Errno::ECONNRESET
Errno::ENOBUFS
Errno::EISCONN
Errno::ENOTCONN
Errno::ESHUTDOWN
Errno::ETOOMANYREFS
Errno::ETIMEDOUT
Errno::ECONNREFUSED
Errno::EHOSTDOWN
Errno::EHOSTUNREACH
Errno::EALREADY
Errno::EINPROGRESS
Errno::ESTALE
Errno::EDQUOT
Errno::ECANCELED
Errno::ENOTRECOVERABLE
Errno::EOWNERDEAD
Errno::EAUTH
Errno::EBADRPC
Errno::EFTYPE
Errno::ENEEDAUTH
Errno::ENOATTR
Errno::ENOTSUP
Errno::EPROCLIM
Errno::EPROCUNAVAIL
Errno::EPROGMISMATCH
Errno::EPROGUNAVAIL
Errno::ERPCMISMATCH
RegexpError
Encoding::UndefinedConversionError
Encoding::InvalidByteSequenceError
Encoding::ConverterNotFoundError
IOError
EOFError
IO::EAGAINWaitReadable
IO::EAGAINWaitWritable
IO::EINPROGRESSWaitReadable
IO::EINPROGRESSWaitWritable
LocalJumpError
Math::DomainError
StopIteration
ThreadError
FiberError
Rake::Win32::Win32HomeError
Gem::Ext::BuildError
Rake::TaskArgumentError
Rake::RuleRecursionOverflowError
TZInfo::InvalidZoneinfoDirectory
TZInfo::ZoneinfoDirectoryNotFound
```

其它：

```ruby
# identified_by 里面声明一下。
ActionCable UnauthorizedError


```
