require 'bundler/setup'
task default: :test

desc "Test examples in spec files"
task :test do
  sh %(bundle exec common/extract-examples.rb index.html)
end

desc "Extract Examples"
task :examples  do
  sh %(rm -rf examples yaml)
  sh %(bundle exec common/extract-examples.rb --example-dir examples --yaml-dir yaml index.html)
end

desc "Check HTML"
task :check_html do
  require 'nokogiri'
  doc = ::Nokogiri::HTML5(File.open("index.html"), max_parse_errors: 1000)
  unless doc.errors.empty?
    STDERR.puts "Errors found parsing index.html:"
    doc.errors.each {|e| STDERR.puts "  #{e}"}
    exit(1)
  end
end

desc "Create concatenated test manifests for reporting"
file "reports/manifests.nt" do
  require 'rdf'
  require 'json/ld'
  require 'rdf/ntriples'
  graph = RDF::Graph.new do |g|
    %w( https://w3c.github.io/json-ld-streaming/tests/stream-toRdf-manifest.jsonld
    ).each do |man|
      puts "load #{man}"
      local_man = if man.include?('json-ld-streaming')
        basename = File.basename(man)
        File.expand_path("../tests/#{basename}", __FILE__)
      else
        man
      end
      g.load(local_man, base_uri: man, unique_bnodes: true)
    end
  end
  puts "write"
  RDF::NTriples::Writer.open("reports/manifests.nt", unique_bnodes: true, validate: false) {|w| w << graph}
end
