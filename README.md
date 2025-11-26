# GWSS Data Migration
Migration from Hyrax 3.6 to Hyrax 5.x / Fedora 4.75 to Fedora 6.x

## Our approach

1. Use the fcrepo export tool to export our existing Fedora 4.75 repository.
2. Use RDF tools (pyoxigraph) to generate a graph database from exported `.ttl` files.
3. Construct Bulkrax importer batches:
   - Extract collection, work, and fileset metadata from the graph database via Sparql queries.
   - Retrieve object binaries from the Fedora REST API for each original file (latest version).
   - Map RDF predicates to Bulkrax columns.
   - Zip Bulkrax csv & files for mid-sized batches (~100 works).
4. Create a rake task to run a Bulkrax import on each batch.
   - The task should also flag any errors from the import process.
5. Run the task on a fresh Hyrax 5/Fedora 6 repository. 
