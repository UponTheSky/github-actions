# About workflows

## Advanced workflow features

### Storing secrets
- storing secret info in Github and use them as environment variables in workflows
```yml
jobs:
  example-job:
    runs-on: ubuntu-latest
    steps:
      - name: Retrieve secret
        env: 
          super_secret: ${{ secrets.SUPERSECRET }}
        run: |
          example-command "$super_secret"
```

### Creating dependent jobs
- specified by `needs`

```yml
jobs:
  setup: 
    runs-on: ubuntu-latest
    steps:
      - run: ./setup_server.sh
  build:
    needs: setup
    runs-on: ubuntu-latest
    steps:
      - run: ./build_server.sh
```

### Using a matrix
- you can use variables in a single job definition to automatically create multiple job runs,
that are based on the combinations of the variables
- using `strategy` keyword

```yml
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix: 
        node: [12, 14, 16]
    steps:
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node }}¡

```

### Caching dependencies
- once the cache is created, it is available to all workflows in the same directory

```yml
jobs:
  example-job:
    steps:
      - name: Cache node modules
        uses: actions/cache@v3
        env:
          cache-name: cache-node-modules
        with:
          path: ~/.npm
          key: ${{ runner.os }}-build-${{ env.cache-name }}-$${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-build-${{ env.cache-name }}-

```

### Using databases and service containers
- if your job requires a DB or cache service, you can use `services` keyword to create an ephemeral container to host the service
- the resulting container is then available to all steps in that job, and is removed when the job has completed

```yml
jobs:
  container-job:
    runs-on: ubuntu-latest
    container: node:14
    services:
      postgres:
        image: postgres
      steps:
        - name: Check out repository code
          uses: actions/checkout@v3
        - name: Install dependencies
          run: npm ci
        - name: Connect to PostgreSQL
          run: node client.js
          env:
            POSTGRES_HOST: postgres
            POSTGRES_PORT: 5432
```

### Using labels to route workflows
- when you want a particular type of runner to process your job
- a workflow will only run on a runner that has all the labels in the `runs-on` array

```yml
jobs:
  example-job:
    runs-on: [self-hosted, linux, x64, gpu]

```

### Reusing workflows
- you can call one workflow from within another workflow

### Using environments
- configuring environments with protection rule and secrets to control the execution of jobs in a workflow

