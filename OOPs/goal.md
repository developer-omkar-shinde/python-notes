# OOP Concepts Used in Trivelta Backend Services

This document outlines the Object-Oriented Programming (OOP) concepts actively used throughout the Trivelta backend services repository.

---

## 1. Classes and Objects

Core organizational unit for domain models, services, repositories, processors, and handlers.

**Examples:**
- `MtsHandler`, `PaymentProcessor`, `Money`, `Campaign`
- `TicketSubmissionService`, `DynamoDBStreamProcessor`

---

## 2. Encapsulation

Internal state is hidden behind private fields and properties, exposed only through controlled interfaces.

**Examples:**
- `_table` in repositories
- `_handlers`, `_middleware` in `DynamoDBStreamProcessor`
- `_context` in `PaymentLogger`
- `_follow_up_commands` in `BaseCommand`

---

## 3. Abstraction

Abstract base classes define contracts and behavior patterns without exposing implementation details.

**Examples:**
- `BaseRepository` — abstract get/save/delete/list methods
- `BaseService` — abstract validate() method
- `PaymentProcessor` — abstract payment processing interface
- `BaseCommand` — abstract validate() / execute() methods

---

## 4. Inheritance

Concrete classes extend base classes to reuse and specialize behavior.

**Examples:**
- `FinixProcessor(PaymentProcessor)` — payment processor implementation
- `JoinGameCommand(BaseCommand)` — poker command implementation
- `BaseModel(Model, ValidationMixin)` — PynamoDB model with mixins
- `DynamoTicketRepository(TicketRepository)` — concrete repository

---

## 5. Polymorphism

Different classes implement the same interface differently, allowing code to work with different types uniformly.

**Examples:**
- Payment processors (`FinixProcessor`, `StripeProcessor`, etc.) all implement `PaymentProcessor`
- Poker commands all implement `BaseCommand.execute()` differently
- Verification providers: `KycProvider`, `FraudProvider`, `GeolocationProvider`

---

## 6. Protocols (Structural Typing)

Python `Protocol` classes define implicit interfaces for structural subtyping without explicit inheritance.

**Examples:**
- `ComplianceResultRepository(Protocol)` — repository port
- `CasinoSessionRepositoryPort`, `UserAccountPort` — domain ports
- `DynamoDBTable(Protocol)` — DynamoDB abstraction
- `_SecretsManagerClient(Protocol)`, `_DynamoDBClient(Protocol)` — module-private ports
- `SgpCalculationClient(Protocol)` — SGP calculator port

---

## 7. Abstract Base Classes (ABC)

Enforced method contracts using `@abstractmethod` for concrete implementations.

**Examples:**
- `BaseRepository[T, ID]` — generic repository with abstractmethods
- `TicketRepository` — MTS-specific repository contract
- `PaymentProcessor` — payment abstraction
- `VerificationProvider` — verification contract
- `BaseCommand` — command abstraction

---

## 8. Composition

Classes are built by injecting or holding other objects rather than inheriting all behavior.

**Examples:**
- `MtsHandler` composes repository, ticket lifecycle, and services
- `FinixProcessor` composes repositories, KYC service, Evervault client
- `DynamoDBStreamProcessor` composes handlers dict and middleware lists
- `PaymentProcessorFactory` composes payment processor instances

**Benefit:** Favored over deep inheritance; flexible and testable.

---

## 9. Dependency Injection

Dependencies are passed into constructors for testability and loose coupling.

**Examples:**
- `MtsHandler.__init__(repository: TicketRepository)`
- `TCMIntegrationsManager.__init__(..., dynamodb_resource, secrets_client)`
- `FinixProcessor` accepts injected repositories and clients
- Test fixtures inject mocked dependencies

**Benefit:** Enables unit testing without real infrastructure; supports multiple implementations.

---

## 10. Dataclasses

Used for immutable and mutable domain types, DTOs, and configuration objects.

**Examples:**
- `@dataclass BaseEntity` — mutable entities with identity
- `@dataclass(frozen=True) Money` — immutable value object
- `@dataclass CreditNote`, `Campaign` — domain entities
- `@dataclass ProcessingContext` — request context
- `@dataclass(slots=True) Config` — efficient configuration

---

## 11. Value Objects

Immutable, identity-free domain types that are compared by value, not reference.

**Examples:**
- `Money` — amount with currency, `is_positive`, `is_zero` properties
- `DepositRequest`, `DepositResult` — payment domain values
- `TicketStatus(StrEnum)` — domain status enumeration
- `ProgressState(StrEnum)` — campaign progress enumeration

---

## 12. Entities

Domain objects with persistent identity, representing things that matter to the business.

**Examples:**
- `MtsTicket` — ticket with unique ID
- `CreditNote` — accounting entity
- `Campaign` — bonus campaign with identity
- `GameRef` — poker game reference

---

## 13. Aggregates

Domain object clusters with a consistency boundary, enforcing invariants across related state.

**Example:**
- `TicketLifecycle.transition_to()` — enforces valid ticket state transitions and business rules

---

## 14. Domain-Driven Design (DDD) Layering

Repository pattern separates persistence logic from domain logic.

**Layers:**
- **Domain**: Entities, value objects, aggregates, interfaces
- **Repository**: Abstract and concrete persistence implementations
- **Service**: Business logic coordination
- **Handler**: Thin orchestrator wiring services and events

**Examples:**
- `services/mts_service/domain/` — MTS domain layer
- `services/mts_service/repository/` — MTS persistence layer
- `services/mts_service/service/` — MTS business services
- `services/mts_service/handler.py` — MTS event handler

---

## 15. Repository Pattern

Persistence is abstracted behind repository classes, isolating data access logic.

**Examples:**
- `TicketRepository(ABC)` — abstract repository
- `DynamoTicketRepository` — DynamoDB implementation
- `BaseDynamoDBRepository[T]` — generic DynamoDB base
- `EventRepository`, `OutcomeRepository` — sportsbook repositories

**Benefit:** Swap implementations (DynamoDB → RDS, etc.) without changing domain code.

---

## 16. Service Layer

Business logic is centralized in service classes, avoiding logic in handlers or domain entities.

**Examples:**
- `TicketSubmissionService` — ticket submission logic
- `ReofferService` — reoffer logic
- `CancellationService` — cancellation logic
- `KYCService` — KYC verification logic

---

## 17. Factory Pattern

Object creation is centralized and encapsulated, often using registries and conditional logic.

**Examples:**
- `PaymentProcessorFactory` — registry + `get_processor(payment_method, provider)`
- `ProviderServiceFactory` — registry mapping providers to instances
- `Money.zero()` — class method factory for special values

---

## 18. Strategy Pattern

Interchangeable algorithm implementations behind a common interface, selected at runtime.

**Examples:**
- Payment processors (`FinixProcessor`, `StripeProcessor`, etc.)
- Verification providers (different KYC/fraud strategies)
- `PaymentProcessorFactory` selects strategy by payment method

**In code:** `PaymentProcessorFactory` docstring explicitly mentions "strategy pattern".

---

## 19. Command Pattern

Actions are encapsulated as command objects with their data and behavior.

**Examples:**
- `BaseCommand(ABC)` — command base with abstract `validate()` / `execute()`
- `JoinGameCommand(BaseCommand)` — join game command
- `_follow_up_commands` — queued commands for async execution

---

## 20. Mixins

Reusable behavior is shared through mixin classes, enabling multiple inheritance of functionality.

**Examples:**
- `ValidationMixin` — shared validation on poker models
- `TimestampMixin` — adds `created_at`/`updated_at`, overrides `save()`
- `BaseModel(Model, ValidationMixin)` — combines PynamoDB and validation

---

## 21. Enums and String Enums

Used for fixed, type-safe domain states, categories, and options.

**Examples:**
- `TicketStatus(StrEnum)` — ticket states (PENDING, WON, LOST, etc.)
- `ProgressState(StrEnum)` — campaign progress states
- `PaymentMethod(Enum)` — payment method types
- `TransactionStatus(Enum)` — transaction states
- `DynamoDBEventName(StrEnum)` — DynamoDB stream events (INSERT, MODIFY, REMOVE)

---

## 22. Properties

Computed and read-only accessors using `@property` decorator.

**Examples:**
- `Money.is_positive` — computed boolean
- `DepositResult.is_successful` — derived from result type
- `Campaign.amount` — total amount calculation
- `BaseCommand.follow_up_commands` — read-only list of queued commands

---

## 23. Class Methods and Static Methods

Alternative constructors and utility methods.

**Examples:**
- `@classmethod Money.zero()` — factory for zero money
- `@classmethod DynamoDBImage._deserialize_map()` — validator
- `@classmethod GameEvent.from_record()` — parser
- `@staticmethod` helpers in `helper/sgp/shared/stats_utils.py`

---

## 24. Exception Hierarchies

Custom exception classes for domain-specific error handling.

**Examples:**
- `DomainError` → `ResourceNotFoundError`, `ParseError`
- `DepositDeclinedError`, `InvalidPaymentMethodError` — payment errors
- `NonRetryableCommandError`, `InsufficientBalanceError` — command errors
- `HandlerException`, `ProcessingException` — stream processing errors

---

## 25. Context Managers

Object lifecycle and temporary context handling using `@contextmanager` or `__enter__`/`__exit__`.

**Examples:**
- `PaymentLogger.with_context()` — temporarily swap logging context
- `registry_scope()`, `provider_scope()` — manage casino loader buffer scope
- `moto_session()` in test fixtures — manage AWS mock session

---

## 26. Generic Types

Used in abstract classes for reusable, type-safe patterns.

**Examples:**
- `BaseRepository(ABC, Generic[T, ID])` — generic repository
- `BaseDynamoDBRepository(ABC, Generic[T])` — generic DynamoDB repository
- `BaseService[T]` — generic service

---

## 27. Decorators (OOP Cross-Cutting Behavior)

Decorator functions and methods for registration and cross-cutting concerns.

**Examples:**
- `@require_feature_flag("verification.fraud_assessment")` — feature gating
- `@processor.handler(DynamoDBEventName.INSERT)` — stream handler registration
- `@processor.middleware(MiddlewareStage.BEFORE_BATCH)` — middleware registration

---

## 28. Singletons (Module-Level Pattern)

Module-level instances providing global access to shared services.

**Examples:**
- `tcm_integrations_manager` — singleton `TCMIntegrationsManager` instance
- `_factory_instance` in `get_payment_processor_factory()` — cached factory

---

## 29. Handler Classes

Thin orchestrators that wire together domain services, repositories, and handlers.

**Examples:**
- `MtsHandler` — composes repository, lifecycle, and three services
- Stream event handlers — route events to processors

**Principle:** Handlers delegate to services; services contain business logic.

---

## 30. Logging Handler Subclass

Custom `logging.Handler` subclass for specialized logging.

**Example:**
- `CloudWatchSignificantEventHandler(logging.Handler)` — lazy CloudWatch client initialization

---

## Architecture Patterns Summary

| Area | Primary OOP Style |
|------|-------------------|
| `helper/domain/` | Shared ABCs + dataclass entities |
| `services/payment_service/payment/v2/` | ABC strategy, factory, frozen VOs, DI |
| `services/mts_service/` | Full DDD: entity, VO, aggregate, repo ABC, services, handler |
| `services/bonus_platform_service/bonus_service_v2/` | Domain models/VOs + DynamoDB repo inheritance |
| `services/internal_gaming_service/poker_coordinator/` | Command ABC + polymorphic commands |
| `helper/poker/models/` | PynamoDB inheritance + mixins |
| `services/casino_service/` | Ports (Protocol) + frozen `msgspec.Struct` domain types |
| `helper/dynamodb_stream_processor.py` | Class-based processor with decorator registration |

---

## Key Design Principles

1. **Composition over Inheritance** — Most services and handlers compose dependencies rather than inherit from complex hierarchies.
2. **Dependency Injection** — Enables testability and loose coupling.
3. **Domain-Driven Design** — Clear separation of domain, persistence, service, and handler layers.
4. **Protocol-Based Ports** — Structural typing for flexible, testable infrastructure seams.
5. **Feature Flags on Everything** — Decorators gate new behavior without changing class structure.
6. **Immutable Value Objects** — Frozen dataclasses for safe, deterministic domain values.

---

## References

- [AGENTS.md](../AGENTS.md) — Engineering standards including DDD, SOLID, and REST API design
- [CLAUDE.md](../CLAUDE.md) — PR review guidelines
- `services/` — Service-specific implementations
- `helper/domain/` — Shared domain abstractions
- `helper/integrations/` — Integration manager and Secrets Manager abstraction
