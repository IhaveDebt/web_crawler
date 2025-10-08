/**
 * Distributed Web Crawler (Simulated)
 *
 * A TypeScript-based crawler that fetches pages (mocked)
 * and indexes metadata using asynchronous task management.
 */

import crypto from "crypto";

class WebCrawler {
  private visited = new Set<string>();
  private index: Map<string, string[]> = new Map();

  async crawl(url: string, depth = 0) {
    if (depth > 2 || this.visited.has(url)) return;
    this.visited.add(url);

    console.log(`Crawling: ${url}`);
    const links = this.mockFetch(url);
    this.index.set(url, links);

    for (const link of links) {
      await this.crawl(link, depth + 1);
    }
  }

  private mockFetch(url: string): string[] {
    const n = Math.floor(Math.random() * 3);
    return Array.from({ length: n }, () => `https://example.com/${crypto.randomUUID().slice(0, 5)}`);
  }

  printIndex() {
    console.log("\nCrawl Index:");
    for (const [url, links] of this.index.entries()) {
      console.log(`- ${url} → ${links.join(", ") || "(no links)"}`);
    }
  }
}

if (require.main === module) {
  const crawler = new WebCrawler();
  crawler.crawl("https://example.com/root").then(() => crawler.printIndex());
}
