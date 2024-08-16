## 1. Configurar el Gateway con GraphQL
- En NestJS, podemos crear un servicio central o API Gateway usando el módulo @nestjs/graphql. Este servicio actuará como el único punto de entrada para las peticiones GraphQL y se encargará de orquestar las consultas a los microservicios.

- Instalamos las dependencias necesarias:

```bash	
npm install @nestjs/graphql apollo-server-express graphql
```

- Configuramos el módulo GraphQL en el gateway:
    
```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { ApolloDriver, ApolloDriverConfig } from '@nestjs/apollo';
import { join } from 'path';

@Module({
  imports: [
    GraphQLModule.forRoot<ApolloDriverConfig>({
      driver: ApolloDriver,
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
      context: ({ req }) => ({ headers: req.headers }),
    }),
  ],
})
export class AppModule {}
```

## 2. Definimos el Esquema GraphQL

- En NestJS, podemos definir el esquema usando decoradores en las clases. Esto incluye los tipos, consultas y mutaciones
- Un ejemplo basico:

```typescript
import { ObjectType, Field, ID } from '@nestjs/graphql';

@ObjectType()
export class User {
  @Field(() => ID)
  id: string;

  @Field()
  name: string;

  @Field()
  email: string;
}

@Resolver(() => User)
export class UserResolver {
  @Query(() => [User])
  async users() {
    return this.userService.getAllUsers();
  }
}
```

## 3. Implementar resolvers que se conecten a los microservicios

- Cada resolver en GraphQL necesita obtener los datos desde los microservicios. Utilizamos el cliente HTTP de NestJS (HttpModule) para hacer peticiones a las APIs REST de los microservicios.

- Un ejemplo:

```typescript
import { Injectable, HttpService } from '@nestjs/common';
import { Query, Resolver } from '@nestjs/graphql';

@Injectable()
export class UserService {
  constructor(private readonly httpService: HttpService) {}

  async getAllUsers() {
    const response = await this.httpService.get('http://microservice-url/users').toPromise();
    return response.data;
  }
}
```

- Este servicio se utiliza en el resolver para obtener y devolver los datos.

## 4. Configurar Cada Microservicio para Integrarse con el Gateway

- Debemos asegurarnos de que los microservicios expongan sus APIs REST de forma clara y consistente.
- Podemos optar por mantener cada microservicio independiente de GraphQL y solo exponer endpoints REST, o podemos implementar GraphQL directamente en cada uno y usar un esquema federado en el Gateway.

## 5. Implementar un Esquema Federado (Opcional)

- Podemos usar Apollo Federation en NestJS. Cada microservicio tendrá su propio esquema GraphQL y el Gateway se encargará de unirlos:

- Configuramos cada microservicio con Apollo Federation:
    
    ```typescript
    GraphQLModule.forRoot({
        driver: ApolloDriver,
        autoSchemaFile: true,
        federation: true,
    });
    ```
- El Gateway tendrá la configuración para federar estos esquemas.