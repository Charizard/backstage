## API Report File for "@backstage/plugin-catalog-backend"

> Do not edit this file. It is a report generated by [API Extractor](https://api-extractor.com/).

```ts
/// <reference types="node" />

import { Account } from 'aws-sdk/clients/organizations';
import { BitbucketIntegration } from '@backstage/integration';
import { Config } from '@backstage/config';
import { DocumentCollator } from '@backstage/search-common';
import { Entity } from '@backstage/catalog-model';
import { EntityName } from '@backstage/catalog-model';
import { EntityPolicy } from '@backstage/catalog-model';
import { EntityRelationSpec } from '@backstage/catalog-model';
import express from 'express';
import { IndexableDocument } from '@backstage/search-common';
import { JsonObject } from '@backstage/config';
import { JsonValue } from '@backstage/config';
import { Knex } from 'knex';
import { Location as Location_2 } from '@backstage/catalog-model';
import { LocationSpec } from '@backstage/catalog-model';
import { Logger as Logger_2 } from 'winston';
import { Organizations } from 'aws-sdk';
import { PluginDatabaseManager } from '@backstage/backend-common';
import { PluginEndpointDiscovery } from '@backstage/backend-common';
import { ResourceEntityV1alpha1 } from '@backstage/catalog-model';
import { ScmIntegrationRegistry } from '@backstage/integration';
import { ScmIntegrations } from '@backstage/integration';
import { UrlReader } from '@backstage/backend-common';
import { Validators } from '@backstage/catalog-model';

// @public (undocumented)
export type AddLocationResult = {
  location: Location_2;
  entities: Entity[];
};

// @public (undocumented)
export type AnalyzeLocationRequest = {
  location: LocationSpec;
};

// @public (undocumented)
export type AnalyzeLocationResponse = {
  existingEntityFiles: AnalyzeLocationExistingEntity[];
  generateEntities: AnalyzeLocationGenerateEntity[];
};

// @public (undocumented)
export class AnnotateLocationEntityProcessor implements CatalogProcessor {
  constructor(options: Options_2);
  // (undocumented)
  preProcessEntity(
    entity: Entity,
    location: LocationSpec,
    _: CatalogProcessorEmit,
    originLocation: LocationSpec,
  ): Promise<Entity>;
}

// @public (undocumented)
export class AnnotateScmSlugEntityProcessor implements CatalogProcessor {
  constructor(opts: { scmIntegrationRegistry: ScmIntegrationRegistry });
  // (undocumented)
  static fromConfig(config: Config): AnnotateScmSlugEntityProcessor;
  // (undocumented)
  preProcessEntity(entity: Entity, location: LocationSpec): Promise<Entity>;
}

// @public
export class AwsOrganizationCloudAccountProcessor implements CatalogProcessor {
  constructor(options: {
    provider: AwsOrganizationProviderConfig;
    logger: Logger_2;
  });
  // (undocumented)
  extractInformationFromArn(
    arn: string,
  ): {
    accountId: string;
    organizationId: string;
  };
  // (undocumented)
  static fromConfig(
    config: Config,
    options: {
      logger: Logger_2;
    },
  ): AwsOrganizationCloudAccountProcessor;
  // (undocumented)
  getAwsAccounts(): Promise<Account[]>;
  // (undocumented)
  logger: Logger_2;
  // (undocumented)
  mapAccountToComponent(account: Account): ResourceEntityV1alpha1;
  // (undocumented)
  normalizeName(name: string): string;
  // (undocumented)
  organizations: Organizations;
  // (undocumented)
  provider: AwsOrganizationProviderConfig;
  // (undocumented)
  readLocation(
    location: LocationSpec,
    _optional: boolean,
    emit: CatalogProcessorEmit,
  ): Promise<boolean>;
}

// @public (undocumented)
export class BitbucketDiscoveryProcessor implements CatalogProcessor {
  constructor(options: {
    integrations: ScmIntegrationRegistry;
    parser?: BitbucketRepositoryParser;
    logger: Logger_2;
  });
  // (undocumented)
  static fromConfig(
    config: Config,
    options: {
      parser?: BitbucketRepositoryParser;
      logger: Logger_2;
    },
  ): BitbucketDiscoveryProcessor;
  // (undocumented)
  readLocation(
    location: LocationSpec,
    _optional: boolean,
    emit: CatalogProcessorEmit,
  ): Promise<boolean>;
}

// @public (undocumented)
export type BitbucketRepositoryParser = (options: {
  integration: BitbucketIntegration;
  target: string;
  logger: Logger_2;
}) => AsyncIterable<CatalogProcessorResult>;

// @public (undocumented)
export class BuiltinKindsEntityProcessor implements CatalogProcessor {
  // (undocumented)
  postProcessEntity(
    entity: Entity,
    _location: LocationSpec,
    emit: CatalogProcessorEmit,
  ): Promise<Entity>;
  // (undocumented)
  validateEntityKind(entity: Entity): Promise<boolean>;
}

// @public
export class CatalogBuilder {
  constructor(env: CatalogEnvironment);
  addEntityPolicy(...policies: EntityPolicy[]): CatalogBuilder;
  addProcessor(...processors: CatalogProcessor[]): CatalogBuilder;
  build(): Promise<{
    entitiesCatalog: EntitiesCatalog;
    locationsCatalog: LocationsCatalog;
    higherOrderOperation: HigherOrderOperation;
    locationAnalyzer: LocationAnalyzer;
  }>;
  // (undocumented)
  static create(env: CatalogEnvironment): Promise<NextCatalogBuilder>;
  replaceEntityPolicies(policies: EntityPolicy[]): CatalogBuilder;
  replaceProcessors(processors: CatalogProcessor[]): CatalogBuilder;
  setEntityDataParser(parser: CatalogProcessorParser): CatalogBuilder;
  setFieldFormatValidators(validators: Partial<Validators>): CatalogBuilder;
  setPlaceholderResolver(
    key: string,
    resolver: PlaceholderResolver,
  ): CatalogBuilder;
}

// @public (undocumented)
export interface CatalogEntityDocument extends IndexableDocument {
  // (undocumented)
  componentType: string;
  // (undocumented)
  kind: string;
  // (undocumented)
  lifecycle: string;
  // (undocumented)
  namespace: string;
  // (undocumented)
  owner: string;
}

// @public (undocumented)
export interface CatalogProcessingOrchestrator {
  // (undocumented)
  process(request: EntityProcessingRequest): Promise<EntityProcessingResult>;
}

// @public (undocumented)
export type CatalogProcessor = {
  readLocation?(
    location: LocationSpec,
    optional: boolean,
    emit: CatalogProcessorEmit,
    parser: CatalogProcessorParser,
  ): Promise<boolean>;
  preProcessEntity?(
    entity: Entity,
    location: LocationSpec,
    emit: CatalogProcessorEmit,
    originLocation: LocationSpec,
  ): Promise<Entity>;
  validateEntityKind?(entity: Entity): Promise<boolean>;
  postProcessEntity?(
    entity: Entity,
    location: LocationSpec,
    emit: CatalogProcessorEmit,
  ): Promise<Entity>;
  handleError?(
    error: Error,
    location: LocationSpec,
    emit: CatalogProcessorEmit,
  ): Promise<void>;
};

// @public (undocumented)
export type CatalogProcessorEmit = (generated: CatalogProcessorResult) => void;

// @public (undocumented)
export type CatalogProcessorEntityResult = {
  type: 'entity';
  entity: Entity;
  location: LocationSpec;
};

// @public (undocumented)
export type CatalogProcessorErrorResult = {
  type: 'error';
  error: Error;
  location: LocationSpec;
};

// @public (undocumented)
export type CatalogProcessorLocationResult = {
  type: 'location';
  location: LocationSpec;
  optional: boolean;
};

// @public
export type CatalogProcessorParser = (options: {
  data: Buffer;
  location: LocationSpec;
}) => AsyncIterable<CatalogProcessorResult>;

// @public (undocumented)
export type CatalogProcessorRelationResult = {
  type: 'relation';
  relation: EntityRelationSpec;
  entityRef?: string;
};

// @public (undocumented)
export type CatalogProcessorResult =
  | CatalogProcessorLocationResult
  | CatalogProcessorEntityResult
  | CatalogProcessorRelationResult
  | CatalogProcessorErrorResult;

// @public (undocumented)
export class CodeOwnersProcessor implements CatalogProcessor {
  constructor(options: {
    integrations: ScmIntegrations;
    logger: Logger_2;
    reader: UrlReader;
  });
  // (undocumented)
  static fromConfig(
    config: Config,
    options: {
      logger: Logger_2;
      reader: UrlReader;
    },
  ): CodeOwnersProcessor;
  // (undocumented)
  preProcessEntity(entity: Entity, location: LocationSpec): Promise<Entity>;
}

// @public
export class CommonDatabase implements Database {
  constructor(database: Knex, logger: Logger_2);
  // (undocumented)
  addEntities(
    txOpaque: Transaction,
    request: DbEntityRequest[],
  ): Promise<DbEntityResponse[]>;
  // (undocumented)
  addLocation(
    txOpaque: Transaction,
    location: Location_2,
  ): Promise<DbLocationsRow>;
  // (undocumented)
  addLocationUpdateLogEvent(
    locationId: string,
    status: DatabaseLocationUpdateLogStatus,
    entityName?: string | string[],
    message?: string,
  ): Promise<void>;
  // (undocumented)
  entities(
    txOpaque: Transaction,
    request?: DbEntitiesRequest,
  ): Promise<DbEntitiesResponse>;
  // (undocumented)
  entityByName(
    txOpaque: Transaction,
    name: EntityName,
  ): Promise<DbEntityResponse | undefined>;
  // (undocumented)
  entityByUid(
    txOpaque: Transaction,
    uid: string,
  ): Promise<DbEntityResponse | undefined>;
  // (undocumented)
  location(id: string): Promise<DbLocationsRowWithStatus>;
  // (undocumented)
  locationHistory(id: string): Promise<DatabaseLocationUpdateLogEvent[]>;
  // (undocumented)
  locations(): Promise<DbLocationsRowWithStatus[]>;
  // (undocumented)
  removeEntityByUid(txOpaque: Transaction, uid: string): Promise<void>;
  // (undocumented)
  removeLocation(txOpaque: Transaction, id: string): Promise<void>;
  // (undocumented)
  setRelations(
    txOpaque: Transaction,
    originatingEntityId: string,
    relations: EntityRelationSpec[],
  ): Promise<void>;
  // (undocumented)
  transaction<T>(fn: (tx: Transaction) => Promise<T>): Promise<T>;
  // (undocumented)
  updateEntity(
    txOpaque: Transaction,
    request: DbEntityRequest,
    matchingEtag?: string,
    matchingGeneration?: number,
  ): Promise<DbEntityResponse>;
}

// @public (undocumented)
export function createNextRouter(
  options: RouterOptions_2,
): Promise<express.Router>;

// @public (undocumented)
export function createRouter(options: RouterOptions): Promise<express.Router>;

// @public
export type Database = {
  transaction<T>(fn: (tx: Transaction) => Promise<T>): Promise<T>;
  addEntities(
    tx: Transaction,
    request: DbEntityRequest[],
  ): Promise<DbEntityResponse[]>;
  updateEntity(
    tx: Transaction,
    request: DbEntityRequest,
    matchingEtag?: string,
    matchingGeneration?: number,
  ): Promise<DbEntityResponse>;
  entities(
    tx: Transaction,
    request?: DbEntitiesRequest,
  ): Promise<DbEntitiesResponse>;
  entityByName(
    tx: Transaction,
    name: EntityName,
  ): Promise<DbEntityResponse | undefined>;
  entityByUid(
    tx: Transaction,
    uid: string,
  ): Promise<DbEntityResponse | undefined>;
  removeEntityByUid(tx: Transaction, uid: string): Promise<void>;
  setRelations(
    tx: Transaction,
    entityUid: string,
    relations: EntityRelationSpec[],
  ): Promise<void>;
  addLocation(tx: Transaction, location: Location_2): Promise<DbLocationsRow>;
  removeLocation(tx: Transaction, id: string): Promise<void>;
  location(id: string): Promise<DbLocationsRowWithStatus>;
  locations(): Promise<DbLocationsRowWithStatus[]>;
  locationHistory(id: string): Promise<DatabaseLocationUpdateLogEvent[]>;
  addLocationUpdateLogEvent(
    locationId: string,
    status: DatabaseLocationUpdateLogStatus,
    entityName?: string | string[],
    message?: string,
  ): Promise<void>;
};

// @public (undocumented)
export class DatabaseEntitiesCatalog implements EntitiesCatalog {
  constructor(database: Database, logger: Logger_2);
  // (undocumented)
  batchAddOrUpdateEntities(
    requests: EntityUpsertRequest[],
    options?: {
      locationId?: string;
      dryRun?: boolean;
      outputEntities?: boolean;
    },
  ): Promise<EntityUpsertResponse[]>;
  // (undocumented)
  entities(request?: EntitiesRequest): Promise<EntitiesResponse>;
  // (undocumented)
  removeEntityByUid(uid: string): Promise<void>;
}

// @public (undocumented)
export class DatabaseLocationsCatalog implements LocationsCatalog {
  constructor(database: Database);
  // (undocumented)
  addLocation(location: Location_2): Promise<Location_2>;
  // (undocumented)
  location(id: string): Promise<LocationResponse>;
  // (undocumented)
  locationHistory(id: string): Promise<DatabaseLocationUpdateLogEvent[]>;
  // (undocumented)
  locations(): Promise<LocationResponse[]>;
  // (undocumented)
  logUpdateFailure(
    locationId: string,
    error?: Error,
    entityName?: string,
  ): Promise<void>;
  // (undocumented)
  logUpdateSuccess(
    locationId: string,
    entityName?: string | string[],
  ): Promise<void>;
  // (undocumented)
  removeLocation(id: string): Promise<void>;
}

// @public (undocumented)
export class DatabaseManager {
  // (undocumented)
  static createDatabase(
    knex: Knex,
    options?: Partial<CreateDatabaseOptions>,
  ): Promise<Database>;
  // (undocumented)
  static createInMemoryDatabase(): Promise<Database>;
  // (undocumented)
  static createInMemoryDatabaseConnection(): Promise<Knex>;
  // (undocumented)
  static createTestDatabase(): Promise<Database>;
  // (undocumented)
  static createTestDatabaseConnection(): Promise<Knex>;
}

// @public (undocumented)
export type DbEntityRequest = {
  locationId?: string;
  entity: Entity;
  relations: EntityRelationSpec[];
};

// @public (undocumented)
export type DbEntityResponse = {
  locationId?: string;
  entity: Entity;
};

// @public (undocumented)
export class DefaultCatalogCollator implements DocumentCollator {
  constructor({
    discovery,
    locationTemplate,
  }: {
    discovery: PluginEndpointDiscovery;
    locationTemplate?: string;
  });
  // (undocumented)
  protected applyArgsToFormat(
    format: string,
    args: Record<string, string>,
  ): string;
  // (undocumented)
  protected discovery: PluginEndpointDiscovery;
  // (undocumented)
  execute(): Promise<CatalogEntityDocument[]>;
  // (undocumented)
  protected locationTemplate: string;
  // (undocumented)
  readonly type: string;
}

// @public (undocumented)
export class DefaultCatalogProcessingOrchestrator
  implements CatalogProcessingOrchestrator {
  constructor(options: {
    processors: CatalogProcessor[];
    integrations: ScmIntegrationRegistry;
    logger: Logger_2;
    parser: CatalogProcessorParser;
    policy: EntityPolicy;
  });
  // (undocumented)
  process(request: EntityProcessingRequest): Promise<EntityProcessingResult>;
}

// @public
export function durationText(startTimestamp: [number, number]): string;

// @public (undocumented)
export type EntitiesCatalog = {
  entities(request?: EntitiesRequest): Promise<EntitiesResponse>;
  removeEntityByUid(uid: string): Promise<void>;
  batchAddOrUpdateEntities(
    requests: EntityUpsertRequest[],
    options?: {
      locationId?: string;
      dryRun?: boolean;
      outputEntities?: boolean;
    },
  ): Promise<EntityUpsertResponse[]>;
};

// @public
export type EntitiesSearchFilter = {
  key: string;
  matchValueIn?: string[];
};

// @public (undocumented)
function entity(
  atLocation: LocationSpec,
  newEntity: Entity,
): CatalogProcessorResult;

// @public
export type EntityFilter = {
  anyOf: {
    allOf: EntitiesSearchFilter[];
  }[];
};

// @public
export type EntityPagination = {
  limit?: number;
  offset?: number;
  after?: string;
};

// @public (undocumented)
export type EntityProcessingRequest = {
  entity: Entity;
  state: Map<string, JsonObject>;
};

// @public (undocumented)
export type EntityProcessingResult =
  | {
      ok: true;
      state: Map<string, JsonObject>;
      completedEntity: Entity;
      deferredEntities: DeferredEntity[];
      relations: EntityRelationSpec[];
      errors: Error[];
    }
  | {
      ok: false;
      errors: Error[];
    };

// @public (undocumented)
export class FileReaderProcessor implements CatalogProcessor {
  // (undocumented)
  readLocation(
    location: LocationSpec,
    optional: boolean,
    emit: CatalogProcessorEmit,
  ): Promise<boolean>;
}

// @public (undocumented)
function generalError(
  atLocation: LocationSpec,
  message: string,
): CatalogProcessorResult;

// @public
export class GithubDiscoveryProcessor implements CatalogProcessor {
  constructor(options: { integrations: ScmIntegrations; logger: Logger_2 });
  // (undocumented)
  static fromConfig(
    config: Config,
    options: {
      logger: Logger_2;
    },
  ): GithubDiscoveryProcessor;
  // (undocumented)
  readLocation(
    location: LocationSpec,
    _optional: boolean,
    emit: CatalogProcessorEmit,
  ): Promise<boolean>;
}

// @alpha
export class GithubMultiOrgReaderProcessor implements CatalogProcessor {
  constructor(options: {
    integrations: ScmIntegrations;
    logger: Logger_2;
    orgs: GithubMultiOrgConfig;
  });
  // (undocumented)
  static fromConfig(
    config: Config,
    options: {
      logger: Logger_2;
    },
  ): GithubMultiOrgReaderProcessor;
  // (undocumented)
  readLocation(
    location: LocationSpec,
    _optional: boolean,
    emit: CatalogProcessorEmit,
  ): Promise<boolean>;
}

// @public
export class GithubOrgReaderProcessor implements CatalogProcessor {
  constructor(options: { integrations: ScmIntegrations; logger: Logger_2 });
  // (undocumented)
  static fromConfig(
    config: Config,
    options: {
      logger: Logger_2;
    },
  ): GithubOrgReaderProcessor;
  // (undocumented)
  readLocation(
    location: LocationSpec,
    _optional: boolean,
    emit: CatalogProcessorEmit,
  ): Promise<boolean>;
}

// @public (undocumented)
export type HigherOrderOperation = {
  addLocation(
    spec: LocationSpec,
    options?: {
      dryRun?: boolean;
    },
  ): Promise<AddLocationResult>;
  refreshAllLocations(): Promise<void>;
};

// @public
export class HigherOrderOperations implements HigherOrderOperation {
  constructor(
    entitiesCatalog: EntitiesCatalog,
    locationsCatalog: LocationsCatalog,
    locationReader: LocationReader,
    logger: Logger_2,
  );
  addLocation(
    spec: LocationSpec,
    options?: {
      dryRun?: boolean;
    },
  ): Promise<AddLocationResult>;
  refreshAllLocations(): Promise<void>;
}

// @public (undocumented)
function inputError(
  atLocation: LocationSpec,
  message: string,
): CatalogProcessorResult;

// @public (undocumented)
function location_2(
  newLocation: LocationSpec,
  optional: boolean,
): CatalogProcessorResult;

// @public (undocumented)
export type LocationAnalyzer = {
  analyzeLocation(
    location: AnalyzeLocationRequest,
  ): Promise<AnalyzeLocationResponse>;
};

// @public (undocumented)
export class LocationEntityProcessor implements CatalogProcessor {
  constructor(options: Options_3);
  // (undocumented)
  postProcessEntity(
    entity: Entity,
    location: LocationSpec,
    emit: CatalogProcessorEmit,
  ): Promise<Entity>;
}

// @public (undocumented)
export type LocationReader = {
  read(location: LocationSpec): Promise<ReadLocationResult>;
};

// @public
export class LocationReaders implements LocationReader {
  constructor(options: Options);
  // (undocumented)
  read(location: LocationSpec): Promise<ReadLocationResult>;
}

// @public (undocumented)
export type LocationsCatalog = {
  addLocation(location: Location_2): Promise<Location_2>;
  removeLocation(id: string): Promise<void>;
  locations(): Promise<LocationResponse[]>;
  location(id: string): Promise<LocationResponse>;
  locationHistory(id: string): Promise<LocationUpdateLogEvent[]>;
  logUpdateSuccess(
    locationId: string,
    entityName?: string | string[],
  ): Promise<void>;
  logUpdateFailure(
    locationId: string,
    error?: Error,
    entityName?: string,
  ): Promise<void>;
};

// @public
export class NextCatalogBuilder {
  constructor(env: CatalogEnvironment_2);
  addEntityPolicy(...policies: EntityPolicy[]): NextCatalogBuilder;
  addEntityProvider(...providers: EntityProvider[]): NextCatalogBuilder;
  addProcessor(...processors: CatalogProcessor[]): NextCatalogBuilder;
  build(): Promise<{
    entitiesCatalog: EntitiesCatalog;
    locationsCatalog: LocationsCatalog;
    locationAnalyzer: LocationAnalyzer;
    processingEngine: CatalogProcessingEngine;
    locationService: LocationService;
  }>;
  replaceEntityPolicies(policies: EntityPolicy[]): NextCatalogBuilder;
  replaceProcessors(processors: CatalogProcessor[]): NextCatalogBuilder;
  setEntityDataParser(parser: CatalogProcessorParser): NextCatalogBuilder;
  setFieldFormatValidators(validators: Partial<Validators>): NextCatalogBuilder;
  setPlaceholderResolver(
    key: string,
    resolver: PlaceholderResolver,
  ): NextCatalogBuilder;
  setRefreshIntervalSeconds(seconds: number): NextCatalogBuilder;
}

// @public (undocumented)
function notFoundError(
  atLocation: LocationSpec,
  message: string,
): CatalogProcessorResult;

// @public (undocumented)
export function parseEntityYaml(
  data: Buffer,
  location: LocationSpec,
): Iterable<CatalogProcessorResult>;

// @public
export class PlaceholderProcessor implements CatalogProcessor {
  constructor(options: Options_4);
  // (undocumented)
  preProcessEntity(entity: Entity, location: LocationSpec): Promise<Entity>;
}

// @public (undocumented)
export type PlaceholderResolver = (
  params: ResolverParams,
) => Promise<JsonValue>;

// @public (undocumented)
export type ReadLocationEntity = {
  location: LocationSpec;
  entity: Entity;
  relations: EntityRelationSpec[];
};

// @public (undocumented)
export type ReadLocationError = {
  location: LocationSpec;
  error: Error;
};

// @public (undocumented)
export type ReadLocationResult = {
  entities: ReadLocationEntity[];
  errors: ReadLocationError[];
};

// @public
export type RecursivePartial<T> = {
  [P in keyof T]?: T[P] extends (infer U)[]
    ? RecursivePartial<U>[]
    : T[P] extends object
    ? RecursivePartial<T[P]>
    : T[P];
};

// @public (undocumented)
function relation(spec: EntityRelationSpec): CatalogProcessorResult;

declare namespace results {
  export {
    notFoundError,
    inputError,
    generalError,
    location_2 as location,
    entity,
    relation,
  };
}
export { results };

// @public (undocumented)
export interface RouterOptions {
  // (undocumented)
  config: Config;
  // (undocumented)
  entitiesCatalog?: EntitiesCatalog;
  // (undocumented)
  higherOrderOperation?: HigherOrderOperation;
  // (undocumented)
  locationAnalyzer?: LocationAnalyzer;
  // (undocumented)
  locationsCatalog?: LocationsCatalog;
  // (undocumented)
  locationService?: LocationService;
  // (undocumented)
  logger: Logger_2;
}

// @public
export function runPeriodically(fn: () => any, delayMs: number): () => void;

// @public (undocumented)
export class StaticLocationProcessor implements StaticLocationProcessor {
  constructor(staticLocations: LocationSpec[]);
  // (undocumented)
  static fromConfig(config: Config): StaticLocationProcessor;
  // (undocumented)
  readLocation(
    location: LocationSpec,
    _optional: boolean,
    emit: CatalogProcessorEmit,
  ): Promise<boolean>;
}

// @public
export type Transaction = {
  rollback(): Promise<unknown>;
};

// @public (undocumented)
export class UrlReaderProcessor implements CatalogProcessor {
  constructor(options: Options_5);
  // (undocumented)
  readLocation(
    location: LocationSpec,
    optional: boolean,
    emit: CatalogProcessorEmit,
    parser: CatalogProcessorParser,
  ): Promise<boolean>;
}

// (No @packageDocumentation comment for this package)
```